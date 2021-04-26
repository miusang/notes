# Linux重要目录说明



## 1. acpi

ACPI（高级配置和电源接口）支持操作系统设置和控制各个硬件部件。



## 2. asound
ALSA使用/proc/asound目录下的文件保存设备信息并且实现一些控制目的。

* cardX
  对于系统中已经识别的每个声卡都存在对应的cardX目录。

*  cards
  已注册的声卡的列表。

*  devices
  已注册的ALSA设备的列表.

* modules
  已注册声卡驱动的列表，并不是所有为ALSA所加载的内核模块，而是硬件驱动的列表。一行对应一个正在使用的声卡.

* oss
  OSS（Open Sound System）是unix平台上一个统一的音频接口.

*  pcm
  已分配的pcm流的列表。

*  seq
   包含关于音序器的信息

* timers
  ALSA已知的计时器的列表.

*  version

  ALSA子系统模块（或内核）构建的版本和日期。

  

## 3. filesystems

系统支持的文件系统。



## 4. fs

* cifs

  CIFS是实现文件共享服务的一种文件系统。

* ext4

  第四代扩展文件系统（Fourth extended filesystem）是linux系统下的日志文件系统，是ext3文件系统的后继版本。

* jbd2

  jbd的全拼是journaling block driver 文件系统的日志功能，jbd2是ext4文件系统版本。

* nfsd

  nfs的守护进程，负责接收到用户的调用请求后与内核发出请求并得到调用结果响应给用户。

## 5. interrupts

interrupts中的字段依次是逻辑中断号、中断在各CPU上发生的次数，中断所属父设备名称、硬件中断号、中断触发方式(电平或边沿)、中断名称。

```
           CPU0       CPU1       CPU2       CPU3       
 26:    7520290    6505367    6092411    8058138     GICv2  26 Level     arch_timer
 32:       8184          0          0          0     GICv2  32 Level     ttyS0
 33:          1          0          0          0     GICv2  33 Level     1f002800.keypad, RTC_PM, (null)
 37:      57255          0          0          0     GICv2  37 Level     mstar_mci
 38:          1          0          0          0     GICv2  38 Level     Mstar-otg
 39:       3672          0          0          0     GICv2  39 Level     ehci_hcd:usb1
```















































