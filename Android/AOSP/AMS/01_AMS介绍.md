AMS(ActivityManagerService)是Android三大核心功能（其余两个是View，WMS）之一，主要负责系统中四大组件的启动、切换、调度及应用进程的管理和调度等工作，其职责与操作系统中的进程管理和调度模块类似。



------

四大组件：

* activity

  （1）一个Activty通常就是一个单独的屏幕，提供一个界面让用户点击和滑动等。

  （2）Activity之间通过Intent进行通信。

  （3）app中的每一个Activity都必须再AndroidManifest.xml配置文件中声明，否则系统将不识别也不执行该Activity。

* service

  service用于再后台完成用户指定的操作。service的两种启动方式：（1）startService()，该service是由其他组件调用startService()方法启动的，其生命周期与启动它的组件无关。因此，服务需要再完成任务后调用stopSelf()方法停止，或者由其他组件调用stopService()方法停止。（2）bindServce()，该service调用者绑定在一起，一旦调用者退出，该service也会终止。

* content provider

  ContentProvider实现了应用之间的数据共享。

* broadcast receiver

  广播接收者。有两种注册方式：（1）动态注册，其生命周期伴随着注册者（activity）的生命周期。（2）静态注册，其生命周期伴随着系统的生命周期。Android sdk26之后，无法在AndroidManifest.xml中显示声明大部分广播，除了一部分必要的广播，如ACTION_BOOT_COMPLETED（开机自启）。