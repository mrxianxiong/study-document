### jvm command解释

### 一、常用jdk工具（命令行）

1. jps（jvm process status tool）:显示制定系统内所有的虚拟机进程
2. jstat（jvm statistics monitoring tool）:用于手机虚拟机各方面的运行数据
3. jinfo（configuration info for java）:显示虚拟机配置信息
4. jmap（memory map for java）:生成虚拟机的内存快照（heapdump文件）
5. jhat（jvm heap dump browser）:用于分析heapdump文件，他会建立一个http/html服务器，让用户可以再浏览器上查看分析结果
6. jstack（stack trace for java）:显示虚拟机的线程快照

#### 1.1 jps（jvm process status tool）

命令格式：

jps 【options】 【hostid】

##### 参数解释：

第一个参数【options】

-q：显示进程id

-m：显示进程id，主类名称，以及传入main方法的参数

-l：显示进程id，主类全名

-v：显示进程id，主类名称，以及传入jvm的参数

-V：显示进程id，主类名称

【-mlvV】可以任意组合

第二个参数【hostid】

主机或者是服务器的ip，如果不指定，就默认为当前的主机或者是服务器

注意：如果需要查看其它服务器上的jvm进程，需要在代查看的机器上启动jstatd

#### 1.2 jstat（jvm statistics monitoring tool）

https://blog.csdn.net/SongSir001/article/details/90170913

命令格式【options vmid [interval [count]]】 <pid>

##### 参数解释：

第一个参数：options

代表用户希望查询的虚拟机信息，主要分为3类：类装载，垃圾收集和运行期间编译状况，具体选项及作用如下：

-class：显示类加载器行为的统计信息

-compiler：显示有关java Hotspot vm即时编译器行为的 统计信息

-gc：显示有关垃圾收集堆行为的统计信息

-gccapacity：显示有关各个垃圾回收代容量及其相应空间的统计信息

-gccause：显示有关垃圾收集器头估计信息，及上一次和当前（如果适用）垃圾收集事件的原因

-gcnew：显示新生代行为的统计信息

-gcnewcapacity：显示新生代大小及相应空间的统计信息

-gcold：显示有关老年代行为的统计信息和元空间的统计信息

-gcoldcapacity：显示老年代大小的统计信息

-gcmetacapacity：显示元空间大小的统计信息

-gcutil：显示有关垃圾收集统计信息

-printcomplation：显示java Hotspot VM编译方法统计信息

第二个参数：vmid

如果是本地虚拟机京城，vmid和本地虚拟机唯一id是一致的

如果是远程虚拟机进程，那vmid的格式应当是protocol：lvmid[@hostname[:port]/servername]

第三个参数：interval

采样间隔，单位为秒（s）或者毫秒（ms）

默认单位是毫秒，必须为正整数

指定后，该jstat命令将在每个间隔产生其输出

第四个参数：count

要显示的样本数



##### 样例：

```
1.类加载信息

F:\jvm\jvm-command>jstat -class 8324
Loaded  Bytes  Unloaded  Bytes     Time
   610  1231.8        0     0.0       0.09
```

- Loaded：类加载数量

- Bytes：已加载类占用空间大小

- Unloaded：未加载类的数量

- Bytes：未加载类占用的大小

- Time：耗时



```
2.堆垃圾收集统计信息
F:\jvm\jvm-command>jstat -gc 8324
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT
10752.0 10752.0  0.0    0.0   64512.0   6451.3   172032.0     0.0     4480.0 776.9  384.0   76.6       0    0.000   0      0.000    0.000
```

- S0C（年轻代中第一个survivor(辛存区)的容量（字节））
- S1C（年轻代中第二个survivor(辛存区)的容量（字节））
- S0U（年轻代中第一个survivor(辛存区)目前已使用空间（字节））
- S1U（年轻代中第二个survivor（幸存区）目前已使用空间 (字节)）
- EC（年轻代中Eden（伊甸园）的容量 (字节)）
- EU（年轻代中Eden（伊甸园）目前已使用空间 (字节)）
- OC（Old代的容量 (字节)）
- OU（Old代目前已使用空间 (字节)）
- MC（元空间容量）
- MU（Metaspace已使用容量）
- CCSC（压缩类空间容量）
- CCSU（使用的压缩类空间）
- YGC（从应用程序启动到采样时年轻代中gc次数）
- YGCT（从应用程序启动到采样时年轻代中gc所用时间(s)）
- FGC（从应用程序启动到采样时old代(全gc)gc次数）
- FGCT（从应用程序启动到采样时old代(全gc)gc所用时间(s)）
- GCT（从应用程序启动到采样时gc用的总时间(s)）

```
3、垃圾收集统计摘要
F:\jvm\jvm-command>jstat -gcutil 8324
  S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT
  0.00   0.00  10.00   0.00  17.34  19.94      0    0.000     0    0.000    0.000
```

- S0：幸存者空间0利用率占空间当前容量的百分比

- S1：幸存者空间1占空间当前容量的百分比

- E：伊甸园空间利用率占空间当前容量的百分比

- O：老年代空间利用率占空间当前容量的百分比

- M：元空间利用率占空间当前容量的百分比

- CCS：压缩的类空间利用率百分比

- YGC：新生代GC事件的数量

- YGCT：新生代垃圾收集时间

- FGC：完整GC事件的数量

- FGCT：完全垃圾收集时间

- GCT：垃圾收集总时间



#### 1.3 jinfo（configuration info for java）

作用：实时查看和调整虚拟机各项参数

命令格式：jinfo [options] <pid>

##### 参数解释：

第一个参数：options

no option:输出全部的参数和系统属性

-flag name：输出对应名称的参数

-flag [+ | -]name：开启或者关闭对应名称的参数

-flag name=value：设定对应名称的参数

-flags：输出全部的参数

-sysprops：输出系统属性



#### 1.4 jmap（memory map for java）

命令格式：jmap [options] <pid>

作用：是一个多功能的命令，他可以生成java程序的dump文件，也可以查看堆内对象信息，查看classloader的信息以及finalizer队列

##### 参数解释：

第一个参数：options

no option：查看进程的内存映像信息，类似solaris pmap 命令

heap：显示java堆详细信息

histo[:live]：显示堆中对象的统计信息

clstats：打印类加载器信息

finalizerinfo：显示在f-queue队列等待finalizer线程执行finalizer方法的对象

dump:<dump-options>：生成堆转储快照

```
生成堆快照：

jmap -dump:live,format=b,file=F:\jvm\filename 进程id
```



#### 1.5 jhat（jvm heap dump browser）

作用：java自带堆信息查看工具，功能不齐，

命令格式：jhat [options] 堆转储文件 



#### 1.6 jstack（stack trace for java）

命令格式：jstack [options] <pid>

作用：查看或导出java应用程序中线程堆栈信息

##### 参数解释：

第一个参数：options

-F：当线程挂起时，使用jstack -l pid请求不被响应时，轻质输出线程堆栈

-l：除堆栈外，显示关于锁的附加信息，例如ownable synchronizers

-m：可以同时输出java以及c/c++的堆栈信息

##### 演示：

- CPU占用过高：

	1. 使用process explorer工具，找到CPU占用率高的进程的id
	2. 右击该进程，查看属性，在thread选项卡中，找到CPU占用率高的线程id

### 二、常用jdk工具（可视化）

#### 1. jconsole

#####  作用：查看java程序的运行概况，监视垃圾收集器管理的虚拟机内存（堆内存和元空间）的变化趋势，以及监控程序内的线程



#### 2. visual vm





#### 一： 类加载，连接，初始化

1. 类从加载，连接，初始化到卸载整个生命周期

   加载：查找并加载类文件的二进制数据

   连接：将已经读入内存的类的二进制数据合并到jvm运行时环境中去，包含以下步骤：

   ​	1）验证：确保被加载类的正确性

   ​	2）准备：为类的静态变量分配内存，并初始化他们

   ​	3）解析：把常量池中的符号引用转换成直接引用

2. 类加载，类加载器，双亲委派模型