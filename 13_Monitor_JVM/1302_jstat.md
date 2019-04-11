# jstat

Monitors Java Virtual Machine (JVM) statistics. This command is experimental and unsupported.

监控Java虚拟机（JVM）的统计信息。 jstat命令是实验性质的，官方客服不提供技术支持。

## Synopsis

## 概述


```
jstat [ generalOption | outputOptions vmid [ interval[s|ms] [ count ] ]
```

- generalOption

  A single general command-line option -help or -options. See General Options.

  常规选项，只支持单个，如 `-help` 或者 `-options`。 详细信息请参考下文。

- outputOptions

  One or more output options that consist of a single `statOption`, plus any of the `-t`, `-h`, and `-J` options. See [Output Options](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstat.html#BEHIGDGJ).

  输出选项, 可包含单个 `statOption`，加上任意数量的 `-t`，`-h` 和 `-J` 选项。 详细信息请参考下文。

- vmid

  Virtual machine identifier, which is a string that indicates the target JVM. The general syntax is the following:
  
  虚拟机标识符(virtual machine identifier)，是一个字符串， 用于明确指示目标JVM。 一般语法如下：

  `[protocol:][//]lvmid[@hostname[:port]/servername] `
  
  The syntax of the `vmid` string corresponds to the syntax of a URI. The `vmid` string can vary from a simple integer that represents a local JVM to a more complex construction that specifies a communications protocol, port number, and other implementation-specific values. See [Virtual Machine Identifier](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstat.html#BEHBCGCA).

  `vmid`字符串的语法类似于URI。 可以是一个数字，表示本地JVM； 或者是由多个部分组成的复杂结构：通信协议，端口号，以及其他特定实现相关的值。 详细信息请参考下文。

- interval [s|ms]

  Sampling interval in the specified units, seconds (s) or milliseconds (ms). Default units are milliseconds. Must be a positive integer. When specified, the `jstat` command produces its output at each interval.

  采样间隔时间, 指定以秒（s）或毫秒（ms）为单位。 默认单位是毫秒, 必须是正整数。 如果指定该参数，则`jstat`命令以这个时间间隔输出新的内容。

- count

  Number of samples to display. The default value is infinity which causes the `jstat` command to display statistics until the target JVM terminates or the `jstat` command is terminated. This value must be a positive integer.

  要输出的采样次数, 必须是正整数值。 默认值为无穷大(infinity)，也就是 `jstat` 命令会持续输出统计信息，直到目标JVM关闭, 或者`jstat`命令终止。 

## Description

## 说明

The `jstat` command displays performance statistics for an instrumented Java HotSpot VM. The target JVM is identified by its virtual machine identifier, or `vmid` option.

`jstat` 可以输出一个HotSpot虚拟机的性能统计信息。 目标JVM由其虚拟机标识符，或者`vmid`选项来标识。



## Virtual Machine Identifier

## 虚拟机标识符

The syntax of the `vmid` string corresponds to the syntax of a URI:

`vmid`字符串的语法类似于URI:

```
[protocol:][//]lvmid[@hostname[:port]/servername]
```
其中,

- protocol

  The communications protocol. If the *protocol* value is omitted and a host name is not specified, then the default protocol is a platform-specific optimized local protocol. If the *protocol* value is omitted and a host name is specified, then the default protocol is `rmi`.

  通信协议。 如果省略这个值, 也不指定主机名（host name），则默认协议则是该平台的本地优化协议(optimized local protocol)。 如果省略 *protocol* 值但指定了主机名，则默认协议为 `rmi`。

- lvmid

  The local virtual machine identifier for the target JVM. The `lvmid` is a platform-specific value that uniquely identifies a JVM on a system. The `lvmid` is the only required component of a virtual machine identifier. The `lvmid` is typically, but not necessarily, the operating system's process identifier for the target JVM process. You can use the `jps` command to determine the `lvmid`. Also, you can determine the `lvmid` on Solaris, Linux, and OS X platforms with the `ps` command, and on Windows with the Windows Task Manager.

  目标JVM所在机器内部的本地虚拟机标识符（local virtual machine identifier）。 `lvmid` 唯一地标识了操作系统中的一个JVM，各格平台可能不一样。 `lvmid`是虚拟机标识符的唯一必需组件。 `lvmid`通常是目标JVM进程的进程号(但可能有例外)。 
  
  我们可以使用 `jps` 命令来确定 `lvmid`。 在Solaris，Linux和OS X平台上还可以使用 `ps` 命令来确定`lvmid`，在Windows上系统中可以使用任务管理器查看 `lvmid`。

- hostname

  A hostname or IP address that indicates the target host. If the *hostname* value is omitted, then the target host is the local host.

  主机名, 可以是目标主机的域名或IP地址。 如果省略 *hostname* 值，则目标主机就是本机。

- port

  The default port for communicating with the remote server. If the *hostname* value is omitted or the *protocol* value specifies an optimized, local protocol, then the *port* value is ignored. Otherwise, treatment of the `port` parameter is implementation-specific. For the default `rmi`protocol, the port value indicates the port number for the rmiregistry on the remote host. If the *port* value is omitted and the *protocol* value indicates `rmi`, then the default rmiregistry port (1099) is used.

  与远程服务器通信的默认端口。 
  
  如果省略*hostname*值, 或者指定 *protocol* 为本地优化协议，则会忽略 *port* 值。 
  
  否则，`port`参数的默认值由具体平台确定。 如果是默认的`rmi`协议，则表示远程主机上 rmiregistry（rmi注册服务）的端口号。 如果省略 *port* 并且协议为`rmi`，则使用rmiregistry的默认端口号（1099）。

- servername

  The treatment of the `servername` parameter depends on implementation. For the optimized local protocol, this field is ignored. For the `rmi` protocol, it represents the name of the RMI remote object on the remote host.

  `servername`参数的处理取决于实现。 本地优化协议会忽略此字段。 `rmi`协议下则表示远程主机上，RMI远程对象的名称。





## Options

## 命令选项

The `jstat` command supports two types of options, general options and output options. General options cause the `jstat` command to display simple usage and version information. Output options determine the content and format of the statistical output.

`jstat`命令支持两种类型的选项，常规选项和输出选项。 常规选项只用于显示简单的用法和版本信息。 输出选项决定了输出的内容/格式。

All options and their functionality are subject to change or removal in future releases.

所有选项和功能，在新版本的JDK中，都有可能会发生更改/删除。



### General Options

### 常规选项（General Options）


If you specify one of the general options, then you cannot specify any other option or parameter.

如果指定了一个常规选项，则不能再指定其他选项。

- `-help`

  Displays a help message.

  显示帮助消息。

- `-options`

  Displays a list of static options. See Output Options.

  输出支持的选项（options）。 详细信息请参考下文。

使用示例:

```
jstat
jstat -help
jstat -options
```

只有第一个常规选项有效, 后面的被忽略:

```
jstat -help -options -gc 0000
jstat -options -help -gc 0000
```


### Output Options

### 输出选项（Output Options）

If you do not specify a general option, then you can specify output options. Output options determine the content and format of the `jstat` command's output, and consist of a single `statOption`, plus any of the other output options (`-h`, `-t`, and `-J`). The `statOption` must come first.

如果不指定常规选项，则可以指定输出选项。 输出选项决定了输出的内容/格式，由单个`statOption`部分，加上其他输出选项（`-h`，`-t`和`-J`）组成。 而且`statOption`必须放在前面。

Output is formatted as a table, with columns that are separated by spaces. A header row with titles describes the columns. Use the `-h` option to set the frequency at which the header is displayed. Column header names are consistent among the different options. In general, if two options provide a column with the same name, then the data source for the two columns is the same.

输出被格式化为表格(table)，两列之间由空格分隔。 标题行描述了各个列的信息。 使用 `-h` 选项设置标题行出现的频率。 每个列的标题，在各种输出选项中都是一致的。 一般来说，如果两个输出选项的列具有相同的名称，那么这两列的数据来源就是一样的。

Use the `-t` option to display a time stamp column, labeled Timestamp as the first column of output. The Timestamp column contains the elapsed time, in seconds, since the target JVM started. The resolution of the time stamp is dependent on various factors and is subject to variation due to delayed thread scheduling on heavily loaded systems.

使用`-t`选项，在第一列加上时间戳列，列名为Timestamp。 Timestamp列是目标JVM启动后经历的时间（以秒为单位）。 时间戳的精度可能受各种因素的影响，比如在负载很高的机器上, 线程调度造成的延迟可能就不一致。

Use the interval and count parameters to determine how frequently and how many times, respectively, the `jstat` command displays its output.

interval 参数决定了 `jstat` 命令输出的频率，  count 参数决定了 `jstat` 命令输出的次数。

**Note:** Do not to write scripts to parse the `jstat` command's output because the format might change in future releases. If you write scripts that parse `jstat` command output, then expect to modify them for future releases of this tool.

**官方警告：** 最好不要编写脚本来解析`jstat`命令的输出，因为在将来的JDK版本中可能会发生改变。 如果已经编写了解析脚本，那么最好也为新版本的JDK做好修改的准备。 蕴含的意思是说，最好花钱买JDK11以后提供的全家桶工具包。


- **-statOption**

  Determines the statistics information the jstat command displays. The following lists the available options. Use the -options general option to display the list of options for a particular platform installation. See Stat Options and Output.

  统计选项决定了 jstat 命令输出哪些统计信息。可以使用常规选项 `-options` 查看支持的选项。 详细信息请参考下文。下面是可用的统计选项列表。 

  `class`: Displays statistics about the behavior of the class loader.

  `class`: 显示 class loader 相关的统计信息。 示例: `jstat -class 21891`

  `compiler`: Displays statistics about the behavior of the Java HotSpot VM Just-in-Time compiler.

  `compiler`: 显示即时编译器(Just-in-Time)相关的统计信息。

  `gc`: Displays statistics about the behavior of the garbage collected heap.

  `gc`: 显示堆内存与GC相关的统计信息。这里的信息不包含各个内存池的最大容量。

  `gccapacity`: Displays statistics about the capacities of the generations and their corresponding spaces.

  `gccapacity`: 显示有关各个内存池相关的统计信息。

  `gccause`: Displays a summary about garbage collection statistics (same as -gcutil), with the cause of the last and current (when applicable) garbage collection events.

  `gccause`: 显示垃圾收集相关的汇总统计信息（与 `-gcutil` 类似），但在最后面加上，上次GC的原因（LGCC）, 和当前GC的原因（GCC, 如果处于GC过程中的话）。 示例: `jstat -gccause -h 5 21891 5s`

  `gcnew`: Displays statistics of the behavior of the new generation.

  `gcnew`: 显示新生代(new generations)行为相关的统计信息。

  `gcnewcapacity`: Displays statistics about the sizes of the new generations and its corresponding spaces.

  `gcnewcapacity`: 显示新生代(new generations)和相关空间大小的统计信息。

  `gcold`: Displays statistics about the behavior of the old generation and metaspace statistics.

  `gcold`: 显示老年代和metaspace内存空间相关的统计信息。

  `gcoldcapacity`: Displays statistics about the sizes of the old generation.

  `gcoldcapacity`: 显示老年代内存空间大小相关的统计信息。

  `gcmetacapacity`: Displays statistics about the sizes of the metaspace.

  `gcmetacapacity`: 显示 metaspace 内存空间大小相关的统计信息。

  `gcuti`l: Displays a summary about garbage collection statistics.

  `gcuti`l: 显示垃圾收集相关的汇总统计信息.

  `printcompilation`: Displays Java HotSpot VM compilation method statistics.

  `printcompilation`: 显示 Java HotSpot VM 方法编译相关的统计信息。


- `-h n`

  Displays a column header every *n* samples (output rows), where *n* is a positive integer. Default value is 0, which displays the column header the first row of data.

  每隔n行数据,打印一次列标题，其中*n*是一个正整数。 默认值为0，只在第一行显示列标题。

- `-t`

  Displays a timestamp column as the first column of output. The time stamp is the time since the start time of the target JVM.

  在第一列加上时间戳列，列名为Timestamp。 Timestamp列是目标JVM启动后经历的时间（以秒为单位）

- `-JjavaOption`

  Passes `javaOption` to the Java application launcher. For example, `-J-Xms48m` sets the startup memory to 48 MB. For a complete list of options, see [`java`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/java.html#CBBFHAJA).

  将JVM启动参数 `javaOption` 传递给应用启动程序. 例如, `-J-Xms48m` 将初始内存设置为 48 MB. 完整的JVM启动参数列表请参考Java命令.



### Stat Options and Output

### 统计选项和输出内容


The following information summarizes the columns that the `jstat` command outputs for each *statOption*.

下面汇总了每个统计选项(statOption)的输出列。

- `-class` *option*

- `-class`选项

  Class loader statistics.

  类加载(Class loader)信息统计.
  
  `Loaded`: Number of classes loaded.

  `Loaded`: 已加载的 class 数量.

  `Bytes`: Number of kBs loaded.

  `Bytes`: 已加载的字节数.

  `Unloaded`: Number of classes unloaded.

  `Unloaded`: 已卸载(unloaded)的 class 数量.

  `Bytes`: Number of Kbytes unloaded.

  `Bytes`: 已卸载的字节数.

  `Time`: Time spent performing class loading and unloading operations.

  `Time`: 执行类加载与类卸载所消耗的时间.

- `-compiler` *option*

- `-compiler`选项

  Java HotSpot VM Just-in-Time compiler statistics.

  Java HotSpot VM 即时编译器(Just-in-Time)相关的统计信息。
  
  `Compiled`: Number of compilation tasks performed.

  `Compiled`: 已执行的编译任务数。
  
  `Failed`: Number of compilations tasks failed.

  `Failed`: 编译任务失败数量.
  
  `Invalid`: Number of compilation tasks that were invalidated.

  `Invalid`: 无效的编译任务数量.
  
  `Time`: Time spent performing compilation tasks.

  `Time`: 执行编译任务花费的总时间.
  
  `FailedType`: Compile type of the last failed compilation.

  `FailedType`: 最后一次编译失败的编译类型.
  
  `FailedMethod`: 最后一次编译失败的Class名称与方法

- `-gc` *option*

- `-gc`选项

  Garbage-collected heap statistics.

  垃圾收集相关的堆内存信息. 示例用法: `jstat -gc -h 10 -t 21891 1s`

  `S0C`: Current survivor space 0 capacity (kB).

  `S0C`: 存活区S0的当前容量(capacity), 单位 kB.

  `S1C`: Current survivor space 1 capacity (kB).

  `S1C`: 存活区S1的当前容量(capacity), 单位 kB.

  `S0U`: Survivor space 0 utilization (kB).

  `S0U`: 存活区S0的使用量(utilization), 单位 kB.

  `S1U`: Survivor space 1 utilization (kB).

  `S1U`: 存活区S1的使用量(utilization), 单位 kB.

  `EC`: Current eden space capacity (kB).

  `EC`: 新生代(Eden区)的当前容量(capacity), 单位 kB.

  `EU`: Eden space utilization (kB).

  `EU`: 新生代(Eden区)的使用量(utilization), 单位 kB.

  `OC`: Current old space capacity (kB).

  `OC`: 老年代(old space)的当前容量(capacity), 单位 kB.

  `OU`: Old space utilization (kB).

  `OU`: 老年代(old space)的使用量(utilization), 单位 kB.

  `MC`: Metaspace capacity (kB).

  `MC`: 元数据区(Metaspace)的容量(capacity), 单位 kB.

  `MU`: Metacspace utilization (kB).

  `MU`: 元数据区(Metaspace)的使用量(utilization), 单位 kB.

  `CCSC`: Compressed class space capacity (kB).

  `CCSC`: 压缩的class空间(Compressed class space)容量, 单位 kB.

  `CCSU`: Compressed class space used (kB).

  `CCSU`: 压缩的class空间(Compressed class space)使用量, 单位 kB.

  `YGC`: Number of young generation garbage collection events.

  `YGC`: 年轻代GC(young generation)的次数。

  `YGCT`: Young generation garbage collection time.

  `YGCT`: 年轻代GC消耗的总时间。

  `FGC`: Number of full GC events.

  `FGC`: Full GC 的次数

  `FGCT`: Full garbage collection time.

  `FGCT`: Full GC 消耗的时间.

  `GCT`: Total garbage collection time.

  `GCT`: 垃圾收集消耗的总时间。

- `-gccapacity` *option*

- `-gccapacity`选项

  Memory pool generation and space capacities.

  内存池分代与各个空间的容量。示例用法: `jstat -gccapacity -h 10 -t 21891 1s`

  `NGCMN`: Minimum new generation capacity (kB).

  `NGCMN`: 年轻代的最小容量, 单位 kB.

  `NGCMX`: Maximum new generation capacity (kB).

  `NGCMX`: 年轻代的最大容量, 单位 kB.

  `NGC`: Current new generation capacity (kB).

  `NGC`: 年轻代的当前容量, 单位 kB.

  `S0C`: Current survivor space 0 capacity (kB).

  `S0C`: 存活区S0的当前容量(capacity), 单位 kB.

  `S1C`: Current survivor space 1 capacity (kB).

  `S1C`: 存活区S1的当前容量(capacity), 单位 kB.

  `EC`: Current eden space capacity (kB).

  `EC`: 新生代(Eden区)的当前容量(capacity), 单位 kB.

  `OGCMN`: Minimum old generation capacity (kB).

  `OGCMN`: 老年代允许的最小容量, 单位 kB.

  `OGCMX`: Maximum old generation capacity (kB).

  `OGCMX`: 老年代的最大容量, 单位 kB.

  `OGC`: Current old generation capacity (kB).

  `OGC`: 此刻老年代空间的大小, 单位 kB.

  `OC`: Current old space capacity (kB).

  `OC`: 老年代(old space)的当前容量(capacity), 单位 kB.

  `MCMN`: Minimum metaspace capacity (kB).

  `MCMN`: 元数据区(Metaspace)的最小容量, 单位 kB.

  `MCMX`: Maximum metaspace capacity (kB).

  `MCMX`: 元数据区(Metaspace)的最大容量, 单位 kB.

  `MC`: Metaspace capacity (kB).

  `MC`: 元数据区(Metaspace)的容量(capacity), 单位 kB.

  `CCSMN`: Compressed class space minimum capacity (kB).

  `CCSMN`: 压缩的class空间(Compressed class space)的最小容量, 单位 kB.

  `CCSMX`: Compressed class space maximum capacity (kB).

  `CCSMX`: 压缩的class空间(Compressed class space)的最大容量, 单位 kB.

  `CCSC`: Compressed class space capacity (kB).

  `CCSC`: 压缩的class空间(Compressed class space)容量, 单位 kB.

  `YGC`: Number of young generation GC events.

  `YGC`: 年轻代GC(young generation)的次数。

  `FGC`: Number of full GC events.

  `FGC`: Full GC 的次数

- `-gccause` *option*

- `-gccause`选项

  This option displays the same summary of garbage collection statistics as the `-gcutil` option, but includes the causes of the last garbage collection event and (when applicable) the current garbage collection event. In addition to the columns listed for `-gcutil`, this option adds the following columns.

  此选项展示垃圾收集相关区域的统计信息， 和 `-gcutil` 选项（gc相关使用率）的输出基本一致，但多了上次GC事件的原因(`LGCC`), 以及本次GC（如果正在GC中）的原因。 本选项比`-gcutil`选项多出的列：

  `LGCC`: Cause of last garbage collection

  `LGCC`: 上次GC事件的原因
  
  `GCC`: Cause of current garbage collection

  `GCC`: 本次GC的原因

- `-gcnew` *option*

- `-gcnew`选项

  New generation statistics.

  年轻代的统计信息. （New = Eden + S0 + S1）

  `S0C`: Current survivor space 0 capacity (kB).

  `S0C`: 存活区S0的当前容量(capacity), 单位 kB.

  `S1C`: Current survivor space 1 capacity (kB).

  `S1C`: 存活区S1的当前容量(capacity), 单位 kB.

  `S0U`: Survivor space 0 utilization (kB).

  `S0U`: 存活区S0的使用量(utilization), 单位 kB.

  `S1U`: Survivor space 1 utilization (kB).

  `S1U`: 存活区S1的使用量(utilization), 单位 kB.

  `TT`: Tenuring threshold.

  `TT`: 晋升阈值(Tenuring threshold).

  `MTT`: Maximum tenuring threshold.

  `MTT`: 最大晋升阈值.

  `DSS`: Desired survivor size (kB).

  `DSS`: 期望的存活区大小, 单位 kB.

  `EC`: Current eden space capacity (kB).

  `EC`: 新生代(Eden区)的当前容量(capacity), 单位 kB.

  `EU`: Eden space utilization (kB).

  `EU`: 新生代(Eden区)的使用量(utilization), 单位 kB.

  `YGC`: Number of young generation GC events.

  `YGC`: 年轻代GC(young generation)的次数。

  `YGCT`: Young generation garbage collection time.

  `YGCT`: 年轻代GC消耗的总时间。


- `-gcnewcapacity` *option*

- `-gcnewcapacity`选项

  New generation space size statistics.

  年轻代空间大小统计.

  `NGCMN`: Minimum new generation capacity (kB).

  `NGCMN`: 年轻代的最小容量, 单位 kB.

  `NGCMX`: Maximum new generation capacity (kB).

  `NGCMX`: 年轻代的最大容量, 单位 kB.

  `NGC`: Current new generation capacity (kB).

  `NGC`: 年轻代的当前容量, 单位 kB.

  `S0CMX`: Maximum survivor space 0 capacity (kB).

  `S0CMX`: 存活区S0的最大容量, 单位 kB.

  `S0C`: Current survivor space 0 capacity (kB).

  `S0C`: 存活区S0的当前容量(capacity), 单位 kB.

  `S1CMX`: Maximum survivor space 1 capacity (kB).

  `S1CMX`: 存活区S1的最大容量, 单位 kB.

  `S1C`: Current survivor space 1 capacity (kB).

  `S1C`: 存活区S1的当前容量(capacity), 单位 kB.

  `ECMX`: Maximum eden space capacity (kB).

  `ECMX`: 新生代(eden区)的最大容量, 单位 kB.

  `EC`: Current eden space capacity (kB).

  `EC`: 新生代(Eden区)的当前容量(capacity), 单位 kB.

  `YGC`: Number of young generation GC events.

  `YGC`: 年轻代GC(young generation)的次数。

  `FGC`: Number of full GC events.

  `FGC`: Full GC 的次数


- `-gcold` *option*

- `-gcold`选项

  Old generation and metaspace behavior statistics.

  老年代和元数据区的行为统计。

  `MC`: Metaspace capacity (kB).

  `MC`: 元数据区(Metaspace)的容量(capacity), 单位 kB.

  `MU`: Metaspace utilization (kB).

  `MU`: 元数据区(Metaspace)的使用量(utilization), 单位 kB.

  `CCSC`: Compressed class space capacity (kB).

  `CCSC`: 压缩的class空间(Compressed class space)容量, 单位 kB.

  `CCSU`: Compressed class space used (kB).

  `CCSU`: 压缩的class空间(Compressed class space)使用量, 单位 kB.

  `OC`: Current old space capacity (kB).

  `OC`: 老年代(old space)的当前容量(capacity), 单位 kB.

  `OU`: Old space utilization (kB).

  `OU`: 老年代(old space)的使用量(utilization), 单位 kB.

  `YGC`: Number of young generation GC events.

  `YGC`: 年轻代GC(young generation)的次数。

  `FGC`: Number of full GC events.

  `FGC`: Full GC 的次数

  `FGCT`: Full garbage collection time.

  `FGCT`: Full GC 消耗的时间.

  `GCT`: Total garbage collection time.

  `GCT`: 垃圾收集消耗的总时间。


- `-gcoldcapacity` *option*

- `-gcoldcapacity`选项

  Old generation size statistics.

  老年代空间大小统计.

  `OGCMN`: Minimum old generation capacity (kB).

  `OGCMN`: 老年代允许的最小容量, 单位 kB.

  `OGCMX`: Maximum old generation capacity (kB).

  `OGCMX`: 老年代的最大容量, 单位 kB.

  `OGC`: Current old generation capacity (kB).

  `OGC`: 此刻老年代空间的大小, 单位 kB.

  `OC`: Current old space capacity (kB).

  `OC`: 老年代(old space)的当前容量(capacity), 单位 kB.

  `YGC`: Number of young generation GC events.

  `YGC`: 年轻代GC(young generation)的次数。

  `FGC`: Number of full GC events.

  `FGC`: Full GC 的次数

  `FGCT`: Full garbage collection time.

  `FGCT`: Full GC 消耗的时间.

  `GCT`: Total garbage collection time.

  `GCT`: 垃圾收集消耗的总时间。


- `-gcmetacapacity` *option*

- `-gcmetacapacity`选项

  Metaspace size statistics.

  元数据区大小统计.

  `MCMN`: Minimum metaspace capacity (kB).

  `MCMN`: 元数据区(Metaspace)的最小容量, 单位 kB.

  `MCMX`: Maximum metaspace capacity (kB).

  `MCMX`: 元数据区(Metaspace)的最大容量, 单位 kB.

  `MC`: Metaspace capacity (kB).

  `MC`: 元数据区(Metaspace)的容量(capacity), 单位 kB.

  `CCSMN`: Compressed class space minimum capacity (kB).

  `CCSMN`: 压缩的class空间(Compressed class space)的最小容量, 单位 kB.

  `CCSMX`: Compressed class space maximum capacity (kB).

  `CCSMX`: 压缩的class空间(Compressed class space)的最大容量, 单位 kB.

  `YGC`: Number of young generation GC events.

  `YGC`: 年轻代GC(young generation)的次数。

  `FGC`: Number of full GC events.

  `FGC`: Full GC 的次数

  `FGCT`: Full garbage collection time.

  `FGCT`: Full GC 消耗的时间.

  `GCT`: Total garbage collection time.

  `GCT`: 垃圾收集消耗的总时间。


- `-gcutil` *option*

- `-gcutil`选项

  Summary of garbage collection statistics.

  垃圾收集相关区域的使用率(utilization)统计。

  `S0`: Survivor space 0 utilization as a percentage of the space's current capacity.

  `S0`: 存活区S0, 当前使用量/当前容量, 单位是百分比（%）.

  `S1`: Survivor space 1 utilization as a percentage of the space's current capacity.

  `S1`: 存活区S1, 当前使用量/当前容量, 单位是百分比（%）.

  `E`: Eden space utilization as a percentage of the space's current capacity.

  `E`: Eden区（新生代）, 当前使用量/当前容量, 单位是百分比（%）.

  `O`: Old space utilization as a percentage of the space's current capacity.

  `O`: 老年代, 当前使用量/当前容量, 单位是百分比（%）.

  `M`: Metaspace utilization as a percentage of the space's current capacity.

  `M`: Meta区, 当前使用量/当前容量, 单位是百分比（%）.

  `CCS`: Compressed class space utilization as a percentage.

  `CCS`: 压缩的class空间(Compressed class space)的使用率, 百分比.

  `YGC`: Number of young generation GC events.

  `YGC`: 年轻代GC(young generation)的次数。

  `YGCT`: Young generation garbage collection time.

  `YGCT`: 年轻代GC消耗的总时间。

  `FGC`: Number of full GC events.

  `FGC`: Full GC 的次数

  `FGCT`: Full garbage collection time.

  `FGCT`: Full GC 消耗的时间.

  `GCT`: Total garbage collection time.

  `GCT`: 垃圾收集消耗的总时间。


- `-printcompilation` *option*

- `-printcompilation`选项

  Java HotSpot VM compiler method statistics.

  JVM（HotSpot）编译方法统计。

  `Compiled`: Number of compilation tasks performed by the most recently compiled method.

  `Compiled`: 最近使用的编译方法，已执行的编译任务数。

  `Size`: Number of bytes of byte code of the most recently compiled method.

  `Size`: 最近使用的编译方法，编译的字节码数量(字节数).

  `Type`: Compilation type of the most recently compiled method.

  `Type`: 最近使用的编译方法，执行的编译类型

  `Method`: Class name and method name identifying the most recently compiled method. Class name uses slash (/) instead of dot (.) as a name space separator. Method name is the method within the specified class. The format for these two fields is consistent with the HotSpot `-XX:+PrintCompilation` option.

  `Method`: 标识最近编译方法的类名(Class name)和方法名(method name)。 类名的分隔符使用左斜杠（/）代替点号（.）。 方法名就是指定类中的方法。 这两个字段的格式与JVM启动参数 `-XX:+PrintCompilation` 的格式一致。







## Examples

## jstat使用示例

This section presents some examples of monitoring a local JVM with an *lvmid* of 21891.

下面的示例，假设监控的是本地的JVM， `lvmid`(进程号) 为 `21891`。


### The gcutil Option

This example attaches to lvmid 21891 and takes 7 samples at 250 millisecond intervals and displays the output as specified by the -`gcutil` option.

The output of this example shows that a young generation collection occurred between the third and fourth sample. The collection took 0.078 seconds and promoted objects from the eden space (E) to the old space (O), resulting in an increase of old space utilization from 66.80% to 68.19%. Before the collection, the survivor space was 97.02% utilized, but after this collection it is 91.03% utilized.

```
jstat -gcutil 21891 250 7
  S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT   
  0.00  97.02  70.31  66.80  95.52  89.14      7    0.300     0    0.000    0.300
  0.00  97.02  86.23  66.80  95.52  89.14      7    0.300     0    0.000    0.300
  0.00  97.02  96.53  66.80  95.52  89.14      7    0.300     0    0.000    0.300
 91.03   0.00   1.98  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  15.82  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  17.80  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  17.80  68.19  95.89  91.24      8    0.378     0    0.000    0.378
```





### Repeat the Column Header String

This example attaches to lvmid 21891 and takes samples at 250 millisecond intervals and displays the output as specified by `-gcnew` option. In addition, it uses the `-h3` option to output the column header after every 3 lines of data.

In addition to showing the repeating header string, this example shows that between the second and third samples, a young GC occurred. Its duration was 0.001 seconds. The collection found enough active data that the survivor space 0 utilization (S0U) would have exceeded the desired survivor Size (DSS). As a result, objects were promoted to the old generation (not visible in this output), and the tenuring threshold (TT) was lowered from 31 to 2.

Another collection occurs between the fifth and sixth samples. This collection found very few survivors and returned the tenuring threshold to 31.

```
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
```





### Include a Time Stamp for Each Sample

This example attaches to lvmid 21891 and takes 3 samples at 250 millisecond intervals. The `-t` option is used to generate a time stamp for each sample in the first column.

The Timestamp column reports the elapsed time in seconds since the start of the target JVM. In addition, the `-gcoldcapacity` output shows the old generation capacity (OGC) and the old space capacity (OC) increasing as the heap expands to meet allocation or promotion demands. The old generation capacity (OGC) has grown from 11,696 kB to 13,820 kB after the eighty-first full garbage collection (FGC). The maximum capacity of the generation (and space) is 60,544 kB (OGCMX), so it still has room to expand.

```
Timestamp      OGCMN    OGCMX     OGC       OC       YGC   FGC    FGCT    GCT
          150.1   1408.0  60544.0  11696.0  11696.0   194    80    2.874   3.799
          150.4   1408.0  60544.0  13820.0  13820.0   194    81    2.938   3.863
          150.7   1408.0  60544.0  13820.0  13820.0   194    81    2.938   3.863
```





### Monitor Instrumentation for a Remote JVM

This example attaches to lvmid 40496 on the system named remote.domain using the `-gcutil` option, with samples taken every second indefinitely.

The lvmid is combined with the name of the remote host to construct a *vmid* of `40496@remote.domain`. This vmid results in the use of the `rmi` protocol to communicate to the default `jstatd` server on the remote host. The `jstatd` server is located using the `rmiregistry`command on `remote.domain` that is bound to the default port of the `rmiregistry` command (port 1099).

```
jstat -gcutil 40496@remote.domain 1000
... output omitted
```







See Also


- [`java`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/java.html#CBBFHAJA)

- [`jps`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jps.html#CHDGHCGB)

- [`jstatd`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstatd.html#BABHHDIB)

- [`rmiregistry`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/rmiregistry.html#CHDEDDIE)

<https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstat.html>

