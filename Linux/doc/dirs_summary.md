# Linux重要目录说明

[TOC]



## acpi

ACPI（高级配置和电源接口）支持操作系统设置和控制各个硬件部件。



## asound
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

  

## cgroups

系统支持的cgroup子系统

```
#subsys_name	hierarchy	num_cgroups	enabled
cpuset	5	1	1
cpu	3	1	1
cpuacct	3	1	1
blkio	7	1	1
memory	2	54	1
devices	9	27	1
freezer	11	1	1
net_cls	10	1	1
perf_event	6	1	1
net_prio	10	1	1
hugetlb	4	1	1
pids	8	30	1
rdma	12	1	1
```



## cmdline

内核启动参数，只读。

```
BOOT_IMAGE=/boot/vmlinuz-linux root=UUID=e9dec0fd-9765-46db-89f0-36756117d68c rw loglevel=3 quiet
```



## cpuinfo

提供了cpu的详细信息。



## crypto

 内核支持的加密方式。



## devices

列出字符和块设备的主设备号，以及分配到这些设备号的设备名称。



## diskstats

统计磁盘信息。

```
设备号 编号 设备  读完成次数  合并完成次数   读扇区次数   读操作花费毫秒数   写完成次数   合并写完成次数   写扇区次数   写操作花费的毫秒数   正在处理的输入/输出请求数   输入/输出操作花费的毫秒数   输入/输出操作花费的加权毫秒数。
  11       0 sr0 9 0 3 8 0 0 0 0 0 27 8 0 0 0 0 0 0
   8       0 sda 25073 9474 1411988 10072 77806 52837 1274166 72426 0 64194 82499 0 0 0 0 0 0
   8       1 sda1 161 2027 15998 43 3 0 10 2 0 97 45 0 0 0 0 0 0
   8       2 sda2 58 0 4424 15 0 0 0 0 0 34 15 0 0 0 0 0 0
   8       3 sda3 24796 7447 1389270 9998 77803 52837 1274156 72424 0 64157 82423 0 0 0 0 0 0
```



## dynamic_debug

kernel相关的动态调试功能。



## execdomains

内核当前支持的执行域。



## filesystems

系统支持的文件系统。



## fs

* cifs

  CIFS是实现文件共享服务的一种文件系统。

* ext4

  第四代扩展文件系统（Fourth extended filesystem）是linux系统下的日志文件系统，是ext3文件系统的后继版本。

* jbd2

  jbd的全拼是journaling block driver 文件系统的日志功能，jbd2是ext4文件系统版本。

* nfsd

  nfs的守护进程，负责接收到用户的调用请求后与内核发出请求并得到调用结果响应给用户。

## interrupts

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



## iomem

记录了物理地址的分配情况。



## ioports

IO port空间的地址资源分配情况。















































