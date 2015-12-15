## jstat

Monitors Java Virtual Machine (JVM) statistics. This command is experimental and unsupported.

Synopsis
jstat [ generalOption | outputOptions vmid [ interval[s|ms] [ count ] ]

generalOption
A single general command-line option -help or -options. See General Options.

outputOptions
One or more output options that consist of a single statOption, plus any of the -t, -h, and -J options. See Output Options.

vmid
Virtual machine identifier, which is a string that indicates the target JVM. The general syntax is the following:

[protocol:][//]lvmid[@hostname[:port]/servername]
The syntax of the vmid string corresponds to the syntax of a URI. The vmid string can vary from a simple integer that represents a local JVM to a more complex construction that specifies a communications protocol, port number, and other implementation-specific values. See Virtual Machine Identifier.

interval [s|ms]
Sampling interval in the specified units, seconds (s) or milliseconds (ms). Default units are milliseconds. Must be a positive integer. When specified, the jstat command produces its output at each interval.

count
Number of samples to display. The default value is infinity which causes the jstat command to display statistics until the target JVM terminates or the jstat command is terminated. This value must be a positive integer.

Description
The jstat command displays performance statistics for an instrumented Java HotSpot VM. The target JVM is identified by its virtual machine identifier, or vmid option.

Virtual Machine Identifier
The syntax of the vmid string corresponds to the syntax of a URI:

[protocol:][//]lvmid[@hostname[:port]/servername]
protocol
The communications protocol. If the protocol value is omitted and a host name is not specified, then the default protocol is a platform-specific optimized local protocol. If the protocol value is omitted and a host name is specified, then the default protocol is rmi.

lvmid
The local virtual machine identifier for the target JVM. The lvmid is a platform-specific value that uniquely identifies a JVM on a system. The lvmid is the only required component of a virtual machine identifier. The lvmid is typically, but not necessarily, the operating system's process identifier for the target JVM process. You can use the jps command to determine the lvmid. Also, you can determine the lvmid on Solaris, Linux, and OS X platforms with the ps command, and on Windows with the Windows Task Manager.

hostname
A hostname or IP address that indicates the target host. If the hostname value is omitted, then the target host is the local host.

port
The default port for communicating with the remote server. If the hostname value is omitted or the protocol value specifies an optimized, local protocol, then the port value is ignored. Otherwise, treatment of the port parameter is implementation-specific. For the default rmi protocol, the port value indicates the port number for the rmiregistry on the remote host. If the port value is omitted and the protocol value indicates rmi, then the default rmiregistry port (1099) is used.

servername
The treatment of the servername parameter depends on implementation. For the optimized local protocol, this field is ignored. For the rmi protocol, it represents the name of the RMI remote object on the remote host.

Options
The jstat command supports two types of options, general options and output options. General options cause the jstat command to display simple usage and version information. Output options determine the content and format of the statistical output.

All options and their functionality are subject to change or removal in future releases.

General Options

If you specify one of the general options, then you cannot specify any other option or parameter.

-help
Displays a help message.

-options
Displays a list of static options. See Output Options.

Output Options

If you do not specify a general option, then you can specify output options. Output options determine the content and format of the jstat command's output, and consist of a single statOption, plus any of the other output options (-h, -t, and -J). The statOption must come first.

Output is formatted as a table, with columns that are separated by spaces. A header row with titles describes the columns. Use the -h option to set the frequency at which the header is displayed. Column header names are consistent among the different options. In general, if two options provide a column with the same name, then the data source for the two columns is the same.

Use the -t option to display a time stamp column, labeled Timestamp as the first column of output. The Timestamp column contains the elapsed time, in seconds, since the target JVM started. The resolution of the time stamp is dependent on various factors and is subject to variation due to delayed thread scheduling on heavily loaded systems.

Use the interval and count parameters to determine how frequently and how many times, respectively, the jstat command displays its output.

Note: Do not to write scripts to parse the jstat command's output because the format might change in future releases. If you write scripts that parse jstat command output, then expect to modify them for future releases of this tool.

-statOption
Determines the statistics information the jstat command displays. The following lists the available options. Use the -options general option to display the list of options for a particular platform installation. See Stat Options and Output.

class: Displays statistics about the behavior of the class loader.

compiler: Displays statistics about the behavior of the Java HotSpot VM Just-in-Time compiler.

gc: Displays statistics about the behavior of the garbage collected heap.

gccapacity: Displays statistics about the capacities of the generations and their corresponding spaces.

gccause: Displays a summary about garbage collection statistics (same as -gcutil), with the cause of the last and current (when applicable) garbage collection events.

gcnew: Displays statistics of the behavior of the new generation.

gcnewcapacity: Displays statistics about the sizes of the new generations and its corresponding spaces.

gcold: Displays statistics about the behavior of the old generation and metaspace statistics.

gcoldcapacity: Displays statistics about the sizes of the old generation.

gcmetacapacity: Displays statistics about the sizes of the metaspace.

gcutil: Displays a summary about garbage collection statistics.

printcompilation: Displays Java HotSpot VM compilation method statistics.

-h n
Displays a column header every n samples (output rows), where n is a positive integer. Default value is 0, which displays the column header the first row of data.

-t
Displays a timestamp column as the first column of output. The time stamp is the time since the start time of the target JVM.

-JjavaOption
Passes javaOption to the Java application launcher. For example, -J-Xms48m sets the startup memory to 48 MB. For a complete list of options, see java(1).

Stat Options and Output

The following information summarizes the columns that the jstat command outputs for each statOption.

-class option
Class loader statistics.

Loaded: Number of classes loaded.

Bytes: Number of kBs loaded.

Unloaded: Number of classes unloaded.

Bytes: Number of Kbytes unloaded.

Time: Time spent performing class loading and unloading operations.

-compiler option
Java HotSpot VM Just-in-Time compiler statistics.

Compiled: Number of compilation tasks performed.

Failed: Number of compilations tasks failed.

Invalid: Number of compilation tasks that were invalidated.

Time: Time spent performing compilation tasks.

FailedType: Compile type of the last failed compilation.

FailedMethod: Class name and method of the last failed compilation.

-gc option
Garbage-collected heap statistics.

S0C: Current survivor space 0 capacity (kB).

S1C: Current survivor space 1 capacity (kB).

S0U: Survivor space 0 utilization (kB).

S1U: Survivor space 1 utilization (kB).

EC: Current eden space capacity (kB).

EU: Eden space utilization (kB).

OC: Current old space capacity (kB).

OU: Old space utilization (kB).

MC: Metaspace capacity (kB).

MU: Metacspace utilization (kB).

CCSC: Compressed class space capacity (kB).

CCSU: Compressed class space used (kB).

YGC: Number of young generation garbage collection events.

YGCT: Young generation garbage collection time.

FGC: Number of full GC events.

FGCT: Full garbage collection time.

GCT: Total garbage collection time.

-gccapacity option
Memory pool generation and space capacities.

NGCMN: Minimum new generation capacity (kB).

NGCMX: Maximum new generation capacity (kB).

NGC: Current new generation capacity (kB).

S0C: Current survivor space 0 capacity (kB).

S1C: Current survivor space 1 capacity (kB).

EC: Current eden space capacity (kB).

OGCMN: Minimum old generation capacity (kB).

OGCMX: Maximum old generation capacity (kB).

OGC: Current old generation capacity (kB).

OC: Current old space capacity (kB).

MCMN: Minimum metaspace capacity (kB).

MCMX: Maximum metaspace capacity (kB).

MC: Metaspace capacity (kB).

CCSMN: Compressed class space minimum capacity (kB).

CCSMX: Compressed class space maximum capacity (kB).

CCSC: Compressed class space capacity (kB).

YGC: Number of young generation GC events.

FGC: Number of full GC events.

-gccause option
This option displays the same summary of garbage collection statistics as the -gcutil option, but includes the causes of the last garbage collection event and (when applicable) the current garbage collection event. In addition to the columns listed for -gcutil, this option adds the following columns.

LGCC: Cause of last garbage collection

GCC: Cause of current garbage collection

-gcnew option
New generation statistics.

S0C: Current survivor space 0 capacity (kB).

S1C: Current survivor space 1 capacity (kB).

S0U: Survivor space 0 utilization (kB).

S1U: Survivor space 1 utilization (kB).

TT: Tenuring threshold.

MTT: Maximum tenuring threshold.

DSS: Desired survivor size (kB).

EC: Current eden space capacity (kB).

EU: Eden space utilization (kB).

YGC: Number of young generation GC events.

YGCT: Young generation garbage collection time.

-gcnewcapacity option
New generation space size statistics.

NGCMN: Minimum new generation capacity (kB).

NGCMX: Maximum new generation capacity (kB).

NGC: Current new generation capacity (kB).

S0CMX: Maximum survivor space 0 capacity (kB).

S0C: Current survivor space 0 capacity (kB).

S1CMX: Maximum survivor space 1 capacity (kB).

S1C: Current survivor space 1 capacity (kB).

ECMX: Maximum eden space capacity (kB).

EC: Current eden space capacity (kB).

YGC: Number of young generation GC events.

FGC: Number of full GC events.

-gcold option
Old generation and metaspace behavior statistics.

MC: Metaspace capacity (kB).

MU: Metaspace utilization (kB).

CCSC: Compressed class space capacity (kB).

CCSU: Compressed class space used (kB).

OC: Current old space capacity (kB).

OU: Old space utilization (kB).

YGC: Number of young generation GC events.

FGC: Number of full GC events.

FGCT: Full garbage collection time.

GCT: Total garbage collection time.

-gcoldcapacity option
Old generation size statistics.

OGCMN: Minimum old generation capacity (kB).

OGCMX: Maximum old generation capacity (kB).

OGC: Current old generation capacity (kB).

OC: Current old space capacity (kB).

YGC: Number of young generation GC events.

FGC: Number of full GC events.

FGCT: Full garbage collection time.

GCT: Total garbage collection time.

-gcmetacapacity option
Metaspace size statistics.

MCMN: Minimum metaspace capacity (kB).

MCMX: Maximum metaspace capacity (kB).

MC: Metaspace capacity (kB).

CCSMN: Compressed class space minimum capacity (kB).

CCSMX: Compressed class space maximum capacity (kB).

YGC: Number of young generation GC events.

FGC: Number of full GC events.

FGCT: Full garbage collection time.

GCT: Total garbage collection time.

-gcutil option
Summary of garbage collection statistics.

S0: Survivor space 0 utilization as a percentage of the space's current capacity.

S1: Survivor space 1 utilization as a percentage of the space's current capacity.

E: Eden space utilization as a percentage of the space's current capacity.

O: Old space utilization as a percentage of the space's current capacity.

M: Metaspace utilization as a percentage of the space's current capacity.

CCS: Compressed class space utilization as a percentage.

YGC: Number of young generation GC events.

YGCT: Young generation garbage collection time.

FGC: Number of full GC events.

FGCT: Full garbage collection time.

GCT: Total garbage collection time.

-printcompilation option
Java HotSpot VM compiler method statistics.

Compiled: Number of compilation tasks performed by the most recently compiled method.

Size: Number of bytes of byte code of the most recently compiled method.

Type: Compilation type of the most recently compiled method.

Method: Class name and method name identifying the most recently compiled method. Class name uses slash (/) instead of dot (.) as a name space separator. Method name is the method within the specified class. The format for these two fields is consistent with the HotSpot -XX:+PrintCompilation option.

Examples
This section presents some examples of monitoring a local JVM with an lvmid of 21891.

The gcutil Option

This example attaches to lvmid 21891 and takes 7 samples at 250 millisecond intervals and displays the output as specified by the -gcutil option.

The output of this example shows that a young generation collection occurred between the third and fourth sample. The collection took 0.078 seconds and promoted objects from the eden space (E) to the old space (O), resulting in an increase of old space utilization from 66.80% to 68.19%. Before the collection, the survivor space was 97.02% utilized, but after this collection it is 91.03% utilized.

jstat -gcutil 21891 250 7
  S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT   
  0.00  97.02  70.31  66.80  95.52  89.14      7    0.300     0    0.000    0.300
  0.00  97.02  86.23  66.80  95.52  89.14      7    0.300     0    0.000    0.300
  0.00  97.02  96.53  66.80  95.52  89.14      7    0.300     0    0.000    0.300
 91.03   0.00   1.98  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  15.82  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  17.80  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  17.80  68.19  95.89  91.24      8    0.378     0    0.000    0.378
Repeat the Column Header String

This example attaches to lvmid 21891 and takes samples at 250 millisecond intervals and displays the output as specified by -gcnew option. In addition, it uses the -h3 option to output the column header after every 3 lines of data.

In addition to showing the repeating header string, this example shows that between the second and third samples, a young GC occurred. Its duration was 0.001 seconds. The collection found enough active data that the survivor space 0 utilization (S0U) would have exceeded the desired survivor Size (DSS). As a result, objects were promoted to the old generation (not visible in this output), and the tenuring threshold (TT) was lowered from 31 to 2.

Another collection occurs between the fifth and sixth samples. This collection found very few survivors and returned the tenuring threshold to 31.

jstat -gcnew -h3 21891 250
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
  64.0   64.0    0.0   31.7 31  31   32.0    512.0    178.6    249    0.203
  64.0   64.0    0.0   31.7 31  31   32.0    512.0    355.5    249    0.203
  64.0   64.0   35.4    0.0  2  31   32.0    512.0     21.9    250    0.204
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
  64.0   64.0   35.4    0.0  2  31   32.0    512.0    245.9    250    0.204
  64.0   64.0   35.4    0.0  2  31   32.0    512.0    421.1    250    0.204
  64.0   64.0    0.0   19.0 31  31   32.0    512.0     84.4    251    0.204
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
  64.0   64.0    0.0   19.0 31  31   32.0    512.0    306.7    251    0.204
Include a Time Stamp for Each Sample

This example attaches to lvmid 21891 and takes 3 samples at 250 millisecond intervals. The -t option is used to generate a time stamp for each sample in the first column.

The Timestamp column reports the elapsed time in seconds since the start of the target JVM. In addition, the -gcoldcapacity output shows the old generation capacity (OGC) and the old space capacity (OC) increasing as the heap expands to meet allocation or promotion demands. The old generation capacity (OGC) has grown from 11,696 kB to 13,820 kB after the eighty-first full garbage collection (FGC). The maximum capacity of the generation (and space) is 60,544 kB (OGCMX), so it still has room to expand.

Timestamp      OGCMN    OGCMX     OGC       OC       YGC   FGC    FGCT    GCT
          150.1   1408.0  60544.0  11696.0  11696.0   194    80    2.874   3.799
          150.4   1408.0  60544.0  13820.0  13820.0   194    81    2.938   3.863
          150.7   1408.0  60544.0  13820.0  13820.0   194    81    2.938   3.863
Monitor Instrumentation for a Remote JVM

This example attaches to lvmid 40496 on the system named remote.domain using the -gcutil option, with samples taken every second indefinitely.

The lvmid is combined with the name of the remote host to construct a vmid of 40496@remote.domain. This vmid results in the use of the rmi protocol to communicate to the default jstatd server on the remote host. The jstatd server is located using the rmiregistry command on remote.domain that is bound to the default port of the rmiregistry command (port 1099).

jstat -gcutil 40496@remote.domain 1000
... output omitted
See Also
java(1)

jps(1)

jstatd(1)

rmiregistry(1)


## jstat
监控Java虚拟机(JVM)的统计数据。 这个命令是经验性的和不支持的。

剧情简介
jstat ( generalOption | outputOptions的vmid ( 时间间隔 [s]女士|( 数 ]]

generalOption
一个通用的命令行选项 - 或 选项 。 看到 一般选择 。

outputOptions
一个或多个选项包括一个输出 statOption ,再加上的 - t , - h , - j 选项。 看到 输出选项 。

的vmid
虚拟机的标识符,它是一个字符串,表明目标JVM。 一般的语法如下:

[protocol:][//]lvmid[@hostname[:port]/servername]
的语法 的vmid 字符串对应于一个URI的语法。 的 的vmid 字符串可以从一个简单的整数代表本地JVM指定一个通信协议的一个更复杂的建筑,端口号和其他特定于实现的价值。 看到 虚拟机的标识符 。

时间间隔 [s]女士|
在指定的单位,采样间隔秒或毫秒(女士)。 默认单位是毫秒。 必须是正整数。 当指定时, jstat 在每个间隔命令产生的输出。

数
要显示数量的样品。 默认值是导致无穷 jstat 命令显示在目标JVM终止或统计数据 jstat 命令终止。 这个值必须是一个正整数。

描述
的 jstat 命令将显示一个插装Java HotSpot VM的性能统计值。 在目标JVM是由虚拟机标识符标识,或 的vmid 选择。

虚拟机的标识符
的语法 的vmid 字符串对应于一个URI的语法:

[protocol:][//]lvmid[@hostname[:port]/servername]
协议
通信协议。 如果 协议 价值被忽略,没有指定主机名,然后默认协议是一个特定于平台的优化的本地协议。 如果 协议 值是省略和指定主机名,然后默认协议 rmi 。

lvmid
本地虚拟机标识符为目标JVM。 的 lvmid 是一个特定于平台的价值,唯一地标识一个JVM系统上。 的 lvmid 是唯一的一个虚拟机所需的组件标识符。 的 lvmid 通常是,但不一定,操作系统目标JVM进程的进程标识符。 您可以使用 jps 命令来确定 lvmid 。 同样,你可以确定 lvmid 在Solaris中,Linux和OS X平台的 ps 命令,在Windows和Windows任务管理器。

主机名
一个显示目标主机的主机名或IP地址。 如果 主机名 值被忽略,那么目标主机是本地主机。

港口
默认端口与远程服务器通信。 如果 主机名 省略或价值 协议 指定一个值,优化本地协议,然后 港口 值将被忽略。 否则,治疗的 港口 参数是特定于实现的。 为默认 rmi 协议、端口值表示的端口号rmiregistry在远程主机上。 如果 港口 省略和价值 协议 值表示 rmi ,然后使用默认rmiregistry端口(1099)。

servername
的治疗 servername 参数依赖于实现。 优化的本地协议,这个领域被忽略。 为 rmi 协议,它代表了RMI远程对象的名称在远程主机上。

选项
的 jstat 命令支持两种类型的选项,一般选择和输出选项。 一般选择使 jstat 命令显示简单的使用和版本信息。 输出选项确定的内容和格式统计输出。

所有选项和它们的功能都可能在将来的版本中被改变或删除。

一般选择

如果你指定的选项之一,那么你不能指定其他选项或参数。

-
显示帮助信息。

选项
显示静态的列表选项。 看到 输出选项 。

输出选项

如果你不指定一个通用选项,那么您可以指定输出选项。 输出选项确定的内容和格式 jstat 命令的输出,并由一个单身 statOption ,再加上其他的输出选项( - h , - t , - j )。 的 statOption 必须先来。

输出格式化为一个表,列分隔的空间。 与标题描述列标题行。 使用 - h 选项来设置标题显示的频率。 列标题名称不同的选项中是一致的。 一般来说,如果两个选项提供一个具有相同名称的列,然后两列的数据源是相同的。

使用 - t 选择显示一个时间戳列,时间戳标记的第一列输出。 时间戳列包含运行时间,以秒为单位,因为目标JVM启动。 时间戳的分辨率取决于各种因素,可能变更将在大量线程调度延迟加载系统。

使用间隔和计算参数确定的频率和次数,分别 jstat 命令显示其输出。

注意: 不写脚本解析 jstat 命令的输出,因为格式可能会改变在将来的版本中。 如果你写的脚本解析 jstat 命令输出,然后希望修改他们的未来版本这个工具。

- - - - - - statOption
决定了统计信息 jstat 命令显示。 以下列出了可用的选项。 使用 选项 一般选择显示选项列表中为特定平台安装。 看到 属性选择和输出 。

类 :显示统计信息类装入器的行为。

编译器 :显示统计信息的行为Java HotSpot VM即时编译器。

gc :显示统计信息堆垃圾收集的行为。

gccapacity :显示统计信息的能力代和相应的空间。

gccause :显示一个总结关于垃圾收集统计数据(一样 -gcutil ),最后的原因和当前(适用时)垃圾收集事件。

gcnew :显示统计的新一代的行为。

gcnewcapacity :显示统计信息新一代的大小和其相应的空间。

gcold :显示统计信息的行为,老的一代和metaspace统计数据。

gcoldcapacity :显示统计信息的大小旧的一代。

gcmetacapacity metaspace:显示统计信息的大小。

gcutil :显示一个总结关于垃圾收集统计数据。

printcompilation 统计:显示Java HotSpot VM编译方法。

- h n
显示一个列标题 n 样品(输出行), n 是一个正整数。 默认值是0,这显示列标题第一行的数据。

- t
显示一个时间戳列,第一列的输出。 时间戳是以来目标JVM的启动时间。

- j javaOption
通过 javaOption Java应用程序启动器。 例如, -J-Xms48m 启动内存设置为48 MB。选项的完整列表,看看 java (1) 。

属性选择和输出

以下信息,总结了列 jstat 命令输出为每个 statOption 。

海尔集团 选项
类装入器统计数据。

加载 :装载的类的数量。

字节 :kBs装载的数量。

卸载 :卸载的类的数量。

字节 :Kbytes卸载。

时间 :时间执行类加载和卸载操作。

编译器 选项
Java HotSpot VM即时编译器统计数据。

编译 :执行编译任务的数量。

失败的 :许多编译任务失败了。

无效的 :无效编译任务的数量。

时间 :执行编译任务的时间。

FailedType :编译类型的最后编译失败。

FailedMethod :类名和方法最后编译失败。

gc 选项
堆垃圾收集统计数据。

S0C :目前幸存者空间0能力(kB)。

就是S1C 1:目前幸存者空间容量(kB)。

S0U 0:幸存者空间利用率(kB)。

S1U 1:幸存者空间利用率(kB)。

电子商务 :当前伊甸园空间容量(kB)。

欧盟 :伊甸园空间利用率(kB)。

OC :当前旧空间容量(kB)。

欧 :旧空间利用率(kB)。

MC :Metaspace能力(kB)。

μ :Metacspace利用率(kB)。

CCSC :压缩类空间容量(kB)。

CCSU :使用压缩类空间(kB)。

YGC :许多年轻一代垃圾收集活动。

YGCT :年轻一代垃圾收集时间。

第五代计算机 :数量的完整GC事件。

FGCT :完整的垃圾收集时间。

GCT :总垃圾收集时间。

-gccapacity 选项
内存池的生成和空间能力。

NGCMN :新一代最小容量(kB)。

NGCMX :最大的新一代能力(kB)。

NGC :新一代流容量(kB)。

S0C :目前幸存者空间0能力(kB)。

就是S1C 1:目前幸存者空间容量(kB)。

电子商务 :当前伊甸园空间容量(kB)。

OGCMN 旧的发电能力:最小(kB)。

OGCMX :旧最大发电能力(kB)。

[ :目前旧的发电能力(kB)。

OC :当前旧空间容量(kB)。

MCMN :最低metaspace能力(kB)。

MCMX :最大metaspace容量(kB)。

MC :Metaspace能力(kB)。

CCSMN :压缩类空间最小容量(kB)。

CCSMX :压缩类空间最大容量(kB)。

CCSC :压缩类空间容量(kB)。

YGC :年轻一代的GC事件。

第五代计算机 :数量的完整GC事件。

-gccause 选项
这个选项显示相同的垃圾收集统计数据的总结 -gcutil 选择,但包括最后一个垃圾收集事件的原因和当前垃圾收集事件(适用时)。 除了列出的列 -gcutil ,这个选项增加了下面的列。

LGCC :最后的垃圾收集的原因

海湾合作委员会 :目前的垃圾收集的原因

-gcnew 选项
新一代的统计数据。

S0C :目前幸存者空间0能力(kB)。

就是S1C 1:目前幸存者空间容量(kB)。

S0U 0:幸存者空间利用率(kB)。

S1U 1:幸存者空间利用率(kB)。

TT :任期阈值。

麻省理工 :最大的任期阈值。

DSS :预期幸存者大小(kB)。

电子商务 :当前伊甸园空间容量(kB)。

欧盟 :伊甸园空间利用率(kB)。

YGC :年轻一代的GC事件。

YGCT :年轻一代垃圾收集时间。

-gcnewcapacity 选项
新一代空间大小的统计数据。

NGCMN :新一代最小容量(kB)。

NGCMX :最大的新一代能力(kB)。

NGC :新一代流容量(kB)。

S0CMX :最大的幸存者空间0(kB)的能力。

S0C :目前幸存者空间0能力(kB)。

S1CMX 1:最大的幸存者空间容量(kB)。

就是S1C 1:目前幸存者空间容量(kB)。

ECMX :最大伊甸园空间容量(kB)。

电子商务 :当前伊甸园空间容量(kB)。

YGC :年轻一代的GC事件。

第五代计算机 :数量的完整GC事件。

-gcold 选项
老一辈和metaspace行为统计。

MC :Metaspace能力(kB)。

μ :Metaspace利用率(kB)。

CCSC :压缩类空间容量(kB)。

CCSU :使用压缩类空间(kB)。

OC :当前旧空间容量(kB)。

欧 :旧空间利用率(kB)。

YGC :年轻一代的GC事件。

第五代计算机 :数量的完整GC事件。

FGCT :完整的垃圾收集时间。

GCT :总垃圾收集时间。

-gcoldcapacity 选项
旧一代规模统计数据。

OGCMN 旧的发电能力:最小(kB)。

OGCMX :旧最大发电能力(kB)。

[ :目前旧的发电能力(kB)。

OC :当前旧空间容量(kB)。

YGC :年轻一代的GC事件。

第五代计算机 :数量的完整GC事件。

FGCT :完整的垃圾收集时间。

GCT :总垃圾收集时间。

-gcmetacapacity 选项
Metaspace大小的统计数据。

MCMN :最低metaspace能力(kB)。

MCMX :最大metaspace容量(kB)。

MC :Metaspace能力(kB)。

CCSMN :压缩类空间最小容量(kB)。

CCSMX :压缩类空间最大容量(kB)。

YGC :年轻一代的GC事件。

第五代计算机 :数量的完整GC事件。

FGCT :完整的垃圾收集时间。

GCT :总垃圾收集时间。

-gcutil 选项
垃圾收集统计的总结。

S0 0:幸存者空间利用空间的当前容量的百分比。

S1 :幸存者空间利用空间的当前容量的百分比。

E :伊甸园空间利用率占空间的当前容量。

O :旧空间利用率作为空间的电流容量的百分比。

米 :Metaspace利用率作为空间的电流容量的百分比。

CCS :压缩类空间利用率作为一个百分比。

YGC :年轻一代的GC事件。

YGCT :年轻一代垃圾收集时间。

第五代计算机 :数量的完整GC事件。

FGCT :完整的垃圾收集时间。

GCT :总垃圾收集时间。

-printcompilation 选项
Java HotSpot VM编译器方法统计数据。

编译 :执行编译任务的数量最近编译方法。

大小 :最近的字节的字节码编译方法。

类型 :编译最近编译方法的类型。

方法 :类名和方法名识别最近编译的方法。 类名使用斜杠(/),而不是点(.)作为名称空间分隔符。 方法的名字是指定类中的方法。 这两个字段的格式是一致的热点 - xx:+ PrintCompilation 选择。

例子
本节提供了一些例子的监控本地JVM lvmid 21891股。

gcutil选项

本例高度lvmid 21891,需要7样品每隔250毫秒指定的,并显示输出 gcutil 选择。

这个例子表明,年轻一代的输出样本收集发生在第三和第四。 花费了0.078秒的时间,提升对象集合的伊甸园(E)的旧空间(O),导致增加了旧空间利用率从66.80%降至68.19%。 在收集之前,幸存者空间利用率是97.02%,但在这个集合是91.03%利用。

jstat -gcutil 21891 250 7
  S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT   
  0.00  97.02  70.31  66.80  95.52  89.14      7    0.300     0    0.000    0.300
  0.00  97.02  86.23  66.80  95.52  89.14      7    0.300     0    0.000    0.300
  0.00  97.02  96.53  66.80  95.52  89.14      7    0.300     0    0.000    0.300
 91.03   0.00   1.98  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  15.82  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  17.80  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  17.80  68.19  95.89  91.24      8    0.378     0    0.000    0.378
重复列标题的字符串

本例高度lvmid 21891,需要样品每隔250毫秒和显示指定的输出 -gcnew 选择。 此外,它使用 h3 选择输出列标题后每3行数据。

除了显示重复标题字符串,这个例子显示,第二个和第三个样品,一个年轻的GC。 它的时间是0.001秒。 收集足够的活动数据发现幸存者空间0利用率(S0U)已经超过所需的幸存者大小(DSS)。 因此,对象被提升为旧一代(这个输出中不可见),和任期阈值(TT)降低了从31到2。

另一个集合之间发生第五和第六样本。 这个集合发现很少有幸存者和任期阈值返回31。

jstat -gcnew -h3 21891 250
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
  64.0   64.0    0.0   31.7 31  31   32.0    512.0    178.6    249    0.203
  64.0   64.0    0.0   31.7 31  31   32.0    512.0    355.5    249    0.203
  64.0   64.0   35.4    0.0  2  31   32.0    512.0     21.9    250    0.204
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
  64.0   64.0   35.4    0.0  2  31   32.0    512.0    245.9    250    0.204
  64.0   64.0   35.4    0.0  2  31   32.0    512.0    421.1    250    0.204
  64.0   64.0    0.0   19.0 31  31   32.0    512.0     84.4    251    0.204
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
  64.0   64.0    0.0   19.0 31  31   32.0    512.0    306.7    251    0.204
每个样本包含一个时间戳

本例高度lvmid 21891,需要3样品每隔250毫秒。 的 - t 选项用于生成一个时间戳为每个样本在第一列中。

时间戳列报告运行时间在几秒钟内开始以来的目标JVM。 此外, -gcoldcapacity 输出显示了旧一代能力(OGC)和旧空间能力(OC)增加堆扩展以满足分配或提升的需求。 旧的发电能力(OGC)已从11696 kB到13820 kB八十一后完整的垃圾收集(第五代计算机)。 一代(空间)的最大容量是60544 kB(OGCMX),所以它仍然有扩大的空间。

Timestamp      OGCMN    OGCMX     OGC       OC       YGC   FGC    FGCT    GCT
          150.1   1408.0  60544.0  11696.0  11696.0   194    80    2.874   3.799
          150.4   1408.0  60544.0  13820.0  13820.0   194    81    2.938   3.863
          150.7   1408.0  60544.0  13820.0  13820.0   194    81    2.938   3.863
监测仪表远程JVM

本例高度lvmid 40496名为远程系统上的。 域使用 -gcutil 无限期地与样本选择,每一秒。

lvmid是结合构建一个远程主机的名称 的vmid 的 40496 @remote.domain 。 这导致的vmid使用 rmi 协议默认通信 jstatd 服务器在远程主机上。 的 jstatd 服务器使用 rmiregistry 命令 remote.domain 这是绑定的默认端口 rmiregistry 命令(端口1099)。

jstat -gcutil 40496@remote.domain 1000
... output omitted
另请参阅
java (1)

jps (1)

jstatd (1)

rmiregistry (1)