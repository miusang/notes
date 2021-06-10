我们从SystemServer.main()开始分析：

```java
// 源码路径：frameworks/base/services/java/com/android/server/SystemServer.java
public final class SystemServer {
   /**
    * The main entry point from zygote.
    */
    public static void main(String[] args) {
        new SystemServer().run();
    }

    private void run() {
        // ...
        // Start services.
        try {
            traceBeginAndSlog("StartServices");
            startBootstrapServices();
            startCoreServices();
            startOtherServices();
            SystemServerInitThreadPool.shutdown();
        } catch (Throwable ex) {
            // ...
        }
        // ...
    }

    private void startBootstrapServices() {
        // ...
        // Activity manager runs the show.
        traceBeginAndSlog("StartActivityManager");
        ActivityTaskManagerService atm = mSystemServiceManager.startService(
        ActivityTaskManagerService.Lifecycle.class).getServices();
        mActivityManagerService = ActivityManagerService.Lifecycle.startService(
        mSystemServiceManager, atm);
        mActivityManagerService.setSystemServiceManager(mSystemServiceManager);
        mActivityManagerService.serInstaller(installer);
        mWindowManagerGloballLock = atm.getGlobalLock();
        traceEnd();
        // ...
    }
}
```
### 首先分析构造和启动

可以看到，AMS在启动的时候传入了两个参数：SystemServiceManager和ActivityTaskManagerService。继续分析AMS的启动过程：

```java
// framework/base/services/core/java/com/android/server/am/ActivityManagerService.java
public static final class Lifecycle extends SystemService {
    private final ActivityManagerService mService;
    private static ActivityTaskManagerService sAtm;
    public static ActivityManagerService startService(
        SystemServiceManager ssm, ActivityTaskmanagerService atm) {
        sAtm = atm;
        return ssm.starService(ActivityManagerService.Lifecycle.class).getService();
    }
}

// framework/base/services/core/java/com/android/server/SystemServiceManager.java
public SystemService startService(String className) {
    final Class<SystemService> serviceClass;
    try {
        serviceClass = (Class<SystemService>)Class.forName(className);
    } catch (ClassNotFoundException ex) {
        // ...
    }
    return startService(serviceClass);
}
public <T extends SystemService> T startService(Class<T> serviceClass) {
    // ...
    try {
        Constructor<T> constructor = serviceClass.getConstructor(Context.class);
        service = constructor.newInstance(mContext);
    } catch (...) {
        // ...
    }
    startService(service);
    return service;
}
public void startService(final SystemService service) {
    // Register it.
    mServices.add(service);
    try {
        service.onStart();
    }
}
```
可以看到，上述流程主要在SystemServiceManager中实例化AMS的LifeCycle，继而实例化AMS，然后调用SystemSevice的onStart()来启动AMS。接下来分析AMS的实例化和启动实现。
```java
// framework/base/services/core/java/com/android/server/am/ActivityManagerService.java
public static final class Lifecycle extends SystemService {
    public Lifecycle(conetxt context) {
        super(context);
        // 实例化AMS
        mService = new ActivityManagerService(context, sAtm);
    }
    public void onStart() {
        // 启动AMS
        mService.start();
    }
    /* 值得注意的是Lifecycle这个类名如其意，在Lifecycle会让AMS在系统开机的各个
    阶段做出相应的响应操作 */
    public void onBootPhase(int phase) {
        mService.mBootPhase = phase;
        if (phase == PAHSE_SYETM_SERVICES_READY) {
            mService.mBatterStatsService.systemServicesReady();
            mService.mServices.SystemServicesReady();
        } else if (phase == PHASE_ACTIVITY_MANAGER_READY) {
            mService.startBroadcastObservers();
        } else if (phase == PHASE_THIRD_PARTY_APPS_CAN_START) {
            mService.mPackageWatchdog.onPackagesReady();
        }
    }
}

public ActivityManagerService(Context systemContext, ActivityTaskManagerService atm) {
    // ...
    //AMS的运行上下文与SystemServer一致.
    mContext = systemContext;
    // ...
    // 取出的是ActivityThread的静态变量sCurrentActivityThread.
    // 这意味着mSystemThread与SystemServer中的ActivityThread一致.
    SystemThread = ActivityThread.currentActivityThread();
    mUiContext = mSystemThread.getSystemUiContext();
    
    Slog.i(TAG, "Memory class: " + ActivityManager.staticGetMemoryClass());

    mHandlerThread = new ServiceThread(TAG,
                                       THREAD_PRIORITY_FOREGROUND, false /*allowIo*/);
    mHandlerThread.start();
    // 主要处理AMS的消息.
    mHandler = new MainHandler(mHandlerThread.getLooper());
    // 处理UI线程的消息.
    mUiHandler = mInjector.getUiHandler(this);
    // ...
    // lunig-question AMS的进程列表?
    mProcessList = mInjector.getProcessList(this);
    mProcessList.init(this, activeUids, mPlatformCompat);
    // ...
    // 创建前台、后台、分流广播对象。
    mFgBroadcastQueue = new BroadcastQueue(this, mHandler,
                                           "foreground", foreConstants, false);
    mBgBroadcastQueue = new BroadcastQueue(this, mHandler,
                                           "background", backConstants, true);
    mOffloadBroadcastQueue = new BroadcastQueue(this, mHandler,
                                                "offload", offloadConstants, true);
    mBroadcastQueues[0] = mFgBroadcastQueue;
    mBroadcastQueues[1] = mBgBroadcastQueue;
    mBroadcastQueues[2] = mOffloadBroadcastQueue;
    // ...
    // 挂钩AMT，并调用其initialize方法。
    mActivityTaskManager = atm;
    mActivityTaskManager.initialize(mIntentFirewall, mPendingIntentController,
                                    DisplayThread.get().getLooper());
    // ...
    // 添加看门狗监控
    Watchdog.getInstance().addMonitor(this);
    Watchdog.getInstance().addThread(mHandler);
    // ...
}

private void start() {
    // 移除所有的进程组
    removeAllProcessGroups();
    // 启动cpu监控线程
    mProcessCpuThread.start();
    // 注册电池状态和权限管理服务
    mBatteryStatsService.publish();
    mAppOpsService.publish();
    Slog.d("AppOps", "AppOpsService published");
    LocalServices.addService(ActivityManagerInternal.class, mInternal);
    mActivityTaskManager.onActivityManagerInternalAdded();
    mPendingIntentController.onActivityManagerInternalAdded();
    // Wait for the synchronized block started in mProcessCpuThread,
    // so that any other access to mProcessCpuTracker from main thread
    // will be blocked during mProcessCpuTracker initialization.
    try {
        mProcessCpuInitLatch.await();
    } catch (InterruptedException e) {
        Slog.wtf(TAG, "Interrupted wait during start", e);
        Thread.currentThread().interrupt();
        throw new IllegalStateException("Interrupted wait during start");
    }
}
```

### 接着分析setSystemServiceManager

```java
// ActivityManagerService.java
public void setSystemServiceManager(SystemServiceManager mgr) {
    mSystemServiceManager = mgr;
}
```

可以看到，其作用是在ActivityManagerService实例中添加了SystemService的实例。



















