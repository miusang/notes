Window分为三种类型，分别是应用窗口、子窗口、系统窗口。接下来分析一下，Activity窗口的添加过程。

## 1. Activity的Window创建

熟悉Activity的启动流程都知道，Window的创建过程是在Activity的attach方法中，它在Activity的onCreate()方法前完成一些重要数据的初始化：

```java
// 源码路径：frameworks/base/core/java/android/app/Activity.java
 final void attach(Context context, ActivityThread aThread,
                   Instrumentation instr, IBinder token, int ident,
                   Application application, Intent intent, ActivityInfo info,
                   CharSequence title, Activity parent, String id,
                    NonConfigurationInstances lastNonConfigurationInstances,
                   Configuration config, String referrer, IVoiceInteractor voiceInteractor,
                   Window window, ActivityConfigCallback activityConfigCallback, IBinder assistToken) {
     // ...
     // 1. 创建PhoneWindow
     mWindow = new PhoneWindow(this, window, activityConfigCallback);
     // 2. 设置Window的属性
     mWindow.setWindowControllerCallback(this);
     mWindow.setCallback(this);
     mWindow.setOnWindowDismissedCallback(this);
     mWindow.getLayoutInflater().setPrivateFactory(this);
     if (info.softInputMode != WindowManager.LayoutParams.SOFT_INPUT_STATE_UNSPECIFIED) {
         mWindow.setSoftInputMode(info.softInputMode);
     }
     if (info.uiOptions != 0) {
         mWindow.setUiOptions(info.uiOptions);
     }
     // ...
     // 3. 将Window和WindoManager关联起来
     mWindow.setWindowManager(
         (WindowManager)context.getSystemService(Context.WINDOW_SERVICE),
         mToken, mComponent.flattenToString(),
         (info.flags & ActivityInfo.FLAG_HARDWARE_ACCELERATED) != 0);
     // ...
     mWindowManager = mWindow.getWindowManager();
     // ...
 }
```

接着来看一下Window是如何与WindowManager进行关联的：

```java
// 源码路径：framewoks/base/core/java/android/view/Window.java
public void setWindowManager(WindowManager wm, IBinder appToken, String appName, boolean hardwareAccelerated) {
    // token是Window的重要属性之一，是IBinder类型。
    mAppToken = appToken;
    mAppName = appName; // 应用名
    mHardwareAccelerated = hardwareAccelerated; // 是否硬件加速
    // 获得系统级服务WMS在本地进程的代理
    if (wm == null) {
        wm = (WindowManager)mContext.getSystemService(Context.WINDOW_SERVICE);
    }
    mWindowManager = ((WindowManagerImpl)wm).createLocalWindowManager(this);
}
```



## 2. 视图的创建

在Activity的启动过程中的attach方法里已经完成了Activity的Window的创建和与WindowManager的关联，那么Activity的视图即View是在哪里创建的呢？答案是在我们熟悉的setContentView方法中：

```java
// 源码路径：frameworks/base/core/java/android/app/Activity.java
public void setContentView(@LayoutRes int layoutResID) {
    getWindow().setContentView(layoutResID);
    // ...
}

// 源码路径：frameworks/base/core/java/com/android/internal/policy/PhoneWindow.java
public void setContentView(int layoutResID) {
    // 1. 根据mContentParent是否为空做出不同动作，mContentParent就是上面所讲的id为android.R.id.content的布局，用来set我们id为layoutResID的内容布局
    if (mContentParent == null) {
        // mContentParent为空，创建DecorView，并加载mContentParent
        installDecor();
    } else if (!hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
        // mContentParent不为空，并且没有转场动画，就把mContentParent中的View视图清空，下面会重新加载
        mContentParent.removeAllViews();
    }
    
    // 2. 根据是否有转场动画，做出不同的动作
    if (hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
        // 有转场动画，创建Scene完成转场动画
        final Scene newScene = Scene.getSceneForLayout(mContentParent, layoutResID, getContext());
        transitionTo(newScene);
    } else {
        // 没有转场动画，直接把我们的layoutResID的布局加载进mContentParent
        mLayoutInflater.inflate(layoutResID, mContentParent);
    }
    mContentParent.requestApplyInsets();
    final Callback cb = getCallback();
    if (cb != null && !isDestroyed()) {
        // 触发Activity的onContentChanged方法, 因为Activity实现了这些回调接口
        cb.onContentChanged();
    }
    mContentParentExplicitlySet = true;
}
```

mContentParent开始为null，接着分析installDecor()：

```java
// 源码路径：frameworks/base/core/java/com/android/internal/policy/PhoneWindow.java
private void installDecor() {
    // ...
    // 1. 根据mDecor是否为空，做出不同动作，mDecor就是DecorView，它是继承自FrameLayout
    if (mDecor == null) {
        // mDecor为空，就创建mDecor
        mDecor = generateDecor(-1);
        // ...
    } else {
        // mDecor不为空，不用重复创建，把Window设置给DecorView
        mDecor.setWindow(this);
    }
    if (mContentParent == null) {
        // 如果mContentParent为空，就加载mContentParent
        mContentParent = generateLayout(mDecor);
    }
}
```

接着分析generateDecor()：

```java
// 源码路径：frameworks/base/core/java/com/android/internal/policy/PhoneWindow.java
protected DecorView generateDecor(int featureId) {
    // ...
    // 这里new了一个DecorView
    return new DecorView(context, featureId, this, getAttributes());
}
```

可以看到generateDecor就是简单的创建了一个DecorView并返回，其中this是Window实例，DecorView的构造方法中会把Window设置给DecorView中的mWindow。

接着分析generateLayout()：

```java
// 源码路径：frameworks/base/core/java/com/android/internal/policy/PhoneWindow.java
protected ViewGroup generateLayout(DecorView decor) {
    // 1. 获取到当前的Activity的主题theme的属性，下面忽略的，都是根据theme的属性设置Activity的Window
    TypedArray a = getWindowStyle();
    // ...
    // 2. 这个layoutResource是一个布局id
    int layoutResource;
    // 3. 获得theme的features
    int features = getLocalFeatures();
    // 4. 根据features获得不同的layoutResource
    if ((features & (1 << FEATURE_SWIPE_TO_DISMISS)) != 0) {
        // ... 
    } // ...
    // ...
    // 5. 将上面获取到的layoutResource对应的布局加载进DecorView中
    mDecor.onResourcesLoaded(mLayoutInflater, layoutResource);
    // 6. 因为layoutResource对应的布局已经加载进DecorView中了，所以这里可以通过findViewById获取android.R.id.content的布局
    ViewGroup contentParent = (ViewGroup)findViewById(ID_ANDROID_CONTENT);
    // ...
    // 返回id为android.R.id.content的布局，赋值给mContentParent
    return contentParent;
}
```

generateLayout()这个方法非常长，但是它里面的逻辑很简单，这个方法的主要作用是根据当前的Activity的theme的属性设置Activity的Window，并把根据features获取到的布局加载进传进来的DecorView，并从DecorView中获取android.R.id.content的布局返回给mContentParent。



## 3. 视图的添加

Activity会在handleResumeActivity方法中把DecorView显示出来，而添加一个Winow是通过WindowManager的addView方法实现的，但是Window只是View的载体，并不是真实存在的，所以addView其实就是添加一个View，这个View是依附在Window上，并且这个View是 View Hierarchy 最顶端的根 View，而Activity的的顶级View是DecorView, 所以添加Activity的Window就是添加DecorView。我们来看一下handleResumeActivity方法：

```java
// 源码路径：frameworks/base/core/java/android/app/ActivityThread.java
public void handleResumeActivity(IBinder token, boolean finalStateRequest, boolean isForward, String reason) {
    // 1. ActivityClientRecord里面保存了Activity的信息，同时performResumeActivity会回调Activity的onResume方法
    final ActivityClientRecord r = performResumeActivity(token, finalStateRequest, reason);
    // 2. 得到Activity
    final Activity a = r.activity;
    // ...
    if (r.window == null && !a.mFinished && willBeVisible) {
        // 3. 获取前面Activit创建的Window
        r.window = r.activity.getWindow();
        // 4. 获取前面Window创建的DecorView
        View decor = r.window.getDecorView();
        // 5. 先把DecorView设为不可见
        decor.setVisibility(View.INVISIBLE);
        // 6. Activity关联的WindowManager
        ViewManager wm = a.getWindowManager();
        // 7. 面设置Window的布局参数
        WindowManager.LayoutParams l = r.window.getAttributes();
        a.mDecor = decor;
        // 8. 窗口的类型是TYPE_BASE_APPLICATION，应用类型窗口
        l.type = WindowManager.LayoutParams.TYPE_BASE_APPLICATION;
        // ...
        if (a.mVisibleFromClient) {
            if (!a.mWindowAdded) {
                a.mWindowAdded = true;
                // 9. 调用WindowManager的addView方法
                wm.addView(decor, l);
            } else {
                a.onWindowAttributesChanged(l);
            }
        }
    }
    if (!r.activity.mFinished && willBeVisible && r.activity.mDecor != null && !r.hideForNow) {
        // ...
        if (r.activity.mVisibleFromClient) {
            // 上面已经把Window添加到WMS中了，所以里面会把DecorView显示出来
            r.activity.makeVisible();
        }
        // ...
    }
}
```
