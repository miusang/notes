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
}

private void start() {
}
```

