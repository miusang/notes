## Activity
Activity可以理解为和用户交互的界面。
#### Activity的生命周期
![activity_flow](res/activity_flow.png)
* onCreate
首次创建Activity时调用。可以在这里做一些初始化的工作，如setContenView去加载布局，通过findViewById获得需要的控件，以及初始化数据等。
* onStart
Activity正在被启动，此时Activity已经可见了，但没有显示在前台界面，不能和用户进行交互，比如点击事件。
* onResume
Activity已经前台可见并可以进行交互。
* onPause
Activity正在停止。
* onStop
Activity对用户不可见时调用。
* onDestroy
Activity被销毁前调用。
* onRestart
Activity已停止并即将再次启动前调用。

---
#### Actvity的启动流程
接下来分析一下Activity的启动流程，例如在launcher桌面上点击一个app的图表，launcher进程会响应onClick事件，从而调用startActivity接口启动对应的Activity:
```java
// framework/base/core/java/android/app/Activity.java
public void startActivity(Intent intent) {
    this.startActivity(intent, null);
}

public void startActivity(Intent intent, Bundle options) {
    if (options != null) {
        // ...
    } else {
        startActivityForResult(intent, -1);
    }
}

public void startActivityForResult(Intent intent, int requestCode) {
    startActivityForResult(intent, requestCode, null);
}

public void startActivityForResult(Intent intent, int requestCode, Bundle options) {
    if (mParent == null) {
        // ...
        Instrumentation.ActivityResult ar = mInstrumentation.execStartActivity(this,
            mMainThread.getApplicationThread(), mToken, this, intentm requestCode, options);
        // ...
    }
    // ...
}
```
接下来分析execStartActivity()方法：
```java
// framework/base/core/java/android/app/Instrumentation.java
public ActivityResult execStartActivity(Context who, IBinder contextThread, IBinder token,
    Activity target, Intent intent, int requestCode, Bundle options) {

}
```
