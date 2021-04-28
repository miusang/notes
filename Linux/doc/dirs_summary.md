# Linux重要目录说明

[TOC]



# proc

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



## irq

为每个注册的irq创建一个以irq编号为名字的子目录。每个子目录下的内容：

```
-r--r--r--  affinity_hint 只读条目，用于用户空间做irq平衡只用。
-r--r--r--  effective_affinity
-r--r--r--  effective_affinity_list
-rw-rw-rw-  irq
-r--r--r--  node
-rw-r--r--  smp_affinity    irq和cpu之间的亲缘绑定关系。
-rw-r--r--  smp_affinity_list
-r--r--r--  spurious    可以获得该irq被处理和未被处理的次数的统计信息。
```



## kallsyms

kallsyms抽取了内核用到的所有函数地址(全局的、静态的)和非栈数据变量地址，生成一个数据块，作为只读数据链接进kernel image，相当于内核中存了一个System.map。



## kmsg

打印内核的信息， 但是与dmesg 有不同， 第一次执行/proc/kmsg 打印到目前位置的所有内核信息，再次执行/proc/kmsg，不打印打印过了的信息。



## loadavg

系统平均负载:在特定时间间隔内运行队列中的平均进程数。

```
53.03 52.68 52.59 4/1521 10165
前三个是1、5、15分钟内的平均进程数。第四个的分子是正在运行的进程数，分母是进程总数；最后一个最近运行的进程ID号。
```



## meminfo

系统内存的使用情况。



## modules

系统模块（lsmod)

```
第一列： 模块的名字
第二列： 模块的内存大小，单位是bytes
第三列： 被load的次数，0以为着没有被load过
第四列： 是否依赖第三方moudle，列出这些module
第五列： 模块的状态，有Live， Loading， Unloading三种状态
第六列： 模块当前的内核内存偏移位置。这些信息，debug的时候会非常有用。例如一些诊断工具 oprofile。
```



## mounts

已挂载的设备。



## net

网络相关。

### anycast6

IPv6任播（anycast）地址。在网卡上新配置一个IPv6地址，就会在 /proc/net/anycast6下生成一个对应的任播地址。

### arp

arp缓存表。
```
IP address       HW type     Flags       HW address            Mask     Device
192.168.190.2    0x1         0x2         00:50:56:e2:4b:17     *        ens33
192.168.190.254  0x1         0x2         00:50:56:e8:78:8e     *        ens33
```

### dev
```
 face |bytes    packets errs drop fifo frame compressed multicast|bytes    packets errs drop fifo colls carrier compressed
    lo: 10704135     823    0    0    0     0          0         0 10704135     823    0    0    0     0       0          0
 ens33: 6231658   29913    0    0    0     0          0         0  1447453   15415    0    0    0     0       0          0
 
bytes: The total number of bytes of data transmitted or received by the interface.（接口发送或接收的数据的总字节数）
packets: The total number of packets of data transmitted or received by the interface.（接口发送或接收的数据包总数）
errs: The total number of transmit or receive errors detected by the device driver.（由设备驱动程序检测到的发送或接收错误的总数）
drop: The total number of packets dropped by the device driver.（设备驱动程序丢弃的数据包总数）
fifo: The number of FIFO buffer errors.（FIFO缓冲区错误的数量）
frame: The number of packet framing errors.（分组帧错误的数量）
colls: The number of collisions detected on the interface.（接口上检测到的冲突数）
compressed: The number of compressed packets transmitted or received by the device driver. (This appears to be unused in the 2.2.15 kernel.)（设备驱动程序发送或接收的压缩数据包数）
carrier: The number of carrier losses detected by the device driver.（由设备驱动程序检测到的载波损耗的数量）
multicast: The number of multicast frames transmitted or received by the device driver.（设备驱动程序发送或接收的多播帧数）
```



## partions

系统分区情况。



## pressure

系统压力信息。
avg10、avg60、avg300 分别代表 10s、60s、300s 的时间周期内的阻塞时间百分比。total 是总累计时间，以毫秒为单位。
some 这一行，代表至少有一个任务在某个资源上阻塞的时间占比，full 这一行，代表所有的非idle任务同时被阻塞的时间占比，这期间 cpu 被完全浪费，会带来严重的性能问题。

* cpu
  ```
  some avg10=19.70 avg60=19.72 avg300=20.31 total=1575848274
  ```

* io
  ```
  some avg10=0.00 avg60=0.00 avg300=0.00 total=8720394
  full avg10=0.00 avg60=0.00 avg300=0.00 total=2892637
  ```

* memory
  ```
  some avg10=0.00 avg60=0.00 avg300=0.00 total=185993
  full avg10=0.00 avg60=0.00 avg300=0.00 total=24324
  ```



## sched_debug

每个cpu上的进程调度信息。



## self

我们都知道可以通过/proc/$pid/来获取指定进程的信息，例如内存映射、CPU绑定信息等等。如果某个进程想要获取本进程的系统信息，就可以通过进程的pid来访问/proc/$pid/目录。但是这个方法还需要获取进程pid，在fork、daemon等情况下pid还可能发生变化。为了更方便的获取本进程的信息，linux提供了/proc/self/目录，这个目录比较独特，不同的进程访问该目录时获得的信息是不同的，内容等价于/proc/本进程pid/。进程可以通过访问/proc/self/目录来获取自己的系统信息，而不用每次都获取pid。



## slabinfo

统计slab分配器相关信息。可以用来分析定位内存泄漏问题。

```
slabinfo - version: 2.1
# name            <active_objs> <num_objs> <objsize> <objperslab> <pagesperslab> : tunables <limit> <batchcount> <sharedfactor> : slabdata <active_slabs> <num_slabs> <sharedavail>
fat_inode_cache       21     21    752   21    4 : tunables    0    0    0 : slabdata      1      1      0
fat_cache              0      0     40  102    1 : tunables    0    0    0 : slabdata      0      0      0
fuse_request         208    208    152   26    1 : tunables    0    0    0 : slabdata      8      8      0
fuse_inode        122184 122184    896   36    8 : tunables    0    0    0 : slabdata   3394   3394      0
ext4_groupinfo_4k    364    364    144   28    1 : tunables    0    0    0 : slabdata     13     13      0
ext4_fc_dentry_update      0      0     80   51    1 : tunables    0    0    0 : slabdata      0      0      0
ext4_inode_cache   38583  38583   1192   27    8 : tunables    0    0    0 : slabdata   1429   1429      0
ext4_allocation_context    256    256    128   32    1 : tunables    0    0    0 : slabdata      8      8      0
ext4_io_end          512    512     64   64    1 : tunables    0    0    0 : slabdata      8      8      0
ext4_extent_status  17646  17646     40  102    1 : tunables    0    0    0 : slabdata    173    173      0
jbd2_journal_handle    584    584     56   73    1 : tunables    0    0    0 : slabdata      8      8      0
jbd2_journal_head   3094   3094    120   34    1 : tunables    0    0    0 : slabdata     91     91      0
jbd2_revoke_table_s    256    256     16  256    1 : tunables    0    0    0 : slabdata      1      1      0
jbd2_revoke_record_s   1024   1024     32  128    1 : tunables    0    0    0 : slabdata      8      8      0
```



## softirqs

中断是一种异步的事件处理机制，用来提高系统的并发处理能力。中断事件发生，会触发执行中断处理程序，而中断处理程序被分为上半部和下半部这两个部分。上半部对应硬中断，用来快速处理中断；下半部对应软中断，用来异步处理上半部未完成的工作。Linux 中的软中断包括网络收发、定时、调度、RCU 锁等各种类型，我们可以查看 proc 文件系统中的 /proc/softirqs ，观察软中断的运行情况。在 Linux 中，每个 CPU 都对应一个软中断内核线程，名字是 ksoftirqd/CPU 编号。当软中断事件的频率过高时，内核线程也会因为 CPU 使用率过高而导致软中断处理不及时，进而引发网络收发延迟、调度缓慢等性能问题。



## stat

记录系统进程整体的统计信息。

```
1. cpu信息。
cpu  141099 71 34750 344269176 25593 278639 64424 0 0 0
cpu0 25226 0 4263 42984056 1212 64307 3678 0 0 0
cpu1 19315 4 5668 43018413 7957 36239 3600 0 0 0
cpu2 22605 9 2254 43032104 3583 32410 1421 0 0 0
cpu3 15871 0 10619 43053745 1159 25787 3065 0 0 0
cpu4 20207 9 7785 42933441 4027 62226 49912 0 0 0
cpu5 13011 18 1436 43083025 2623 19249 1057 0 0 0
cpu6 12775 1 1443 43079674 2814 20230 885 0 0 0
cpu7 12085 27 1279 43084714 2214 18187 803 0 0 0
内容的第一行是CPU的整体信息，紧跟着是各个CPU核的信息，各个字段内容如下：
name：指示CPU核
user：用户态花费的时间
nice：nice值为负的进程在用户态所占用的CPU时间
system：内核态占用的CPU时间
idle：空闲时间
iowait：磁盘IO等待的时间
irq：硬中断占用的时间
softirq：软中断占用的时间
steal：如果当前系统运行在虚拟化环境中，则可能会有时间片运行在操作系统上，这个值指的是运行其他操作系统花费的时间
guest：操作系统运行虚拟CPU花费的时间
guest_nice：运行一个带nice值的guest花费的时间

2. 中断信息。
intr 22182190 3 22716 0 0 0 0 0 0 1 0 0 0 55053 0 0 404649 13153 60950 64 242046 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 50671 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
第一个为自系统启动以来，发生的所有中断的次数的总和；之后的每个数对应一个特定的中断自系统启动以来所发生的次数。

3. 自系统启动以来CPU发生的上下文交换的次数。
ctxt 39077563

4. 系统自启动以来，已经运行了的时间，单位为s。
btime 1619096221

5. 自系统启动以来，所创建的任务的数量。
processes 18233

6. 当前正在运行着的任务数量。
procs_running 2

7. 当前被阻塞的任务的数量。
procs_blocked 0

8. 表示从系统启动以来的软中断计数，第一列表示所有软中断的总和；之后各列表示某个指定软中断的数量。
softirq 17510951 1 6252866 86293 242564 268282 0 69288 8317377 0 2274280
```



## swap

swap分区大小及使用情况。

```
Filename				Type		Size		Used		Priority
/dev/sda2                               partition	4194300		0		-2
```



## sys

系统相关，单独说明。



## sysrq-trigger

```
# 立即重新启动计算机
echo "b" > /proc/sysrq-trigger

# 立即关闭计算机
echo "o" > /proc/sysrq-trigger

# 导出内存分配的信息 （可以用/var/log/message 查看）
echo "m" > /proc/sysrq-trigger

# 导出当前CPU寄存器信息和标志位的信息
echo "p" > /proc/sysrq-trigger

# 导出线程状态信息
echo "t" > /proc/sysrq-trigger

# 故意让系统崩溃
echo "c" > /proc/sysrq-trigger

# 立即重新挂载所有的文件系统 
echo "s" > /proc/sysrq-trigger

# 立即重新挂载所有的文件系统为只读
echo "u" > /proc/sysrq-trigger
```



## thread-self

获取指定线程的信息，类似于/proc/self。



## timer_list

内核定时器，打印per_cpu的hrtimer_bases信息以及基于此的timer列表，包括三种时钟MONOTONIC/REALTIME/BOOTTIME；以及Broadcast Tick Device和Per CPU Tick Device信息。



## tty

终端设备信息。



## uptime

```
436156.66 3477968.89
第一个参数num1是代表从系统启动到现在的时间(以秒为单位)
第二个参数num2是代表系统空闲的时间(以秒为单位)
```



## version

系统版本信息。

```
Linux version 5.11.4-arch1-1 (linux@archlinux) (gcc (GCC) 10.2.0, GNU ld (GNU Binutils) 2.36.1) #1 SMP PREEMPT Sun, 07 Mar 2021 18:00:49 +0000
```



## vmallocinfo

虚拟内存分配信息。



## vmstat

虚拟内存统计信息。



## zoneinfo

内存区域使用情况。


























