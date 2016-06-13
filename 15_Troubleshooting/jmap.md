## jmap

Prints shared object memory maps or heap memory details for a process, core file, or remote debug server. This command is experimental and unsupported.

### Synopsis

	jmap [ options ] pid

	jmap [ options ] executable core

	jmap [ options ] [ pid ] server-id@ ] remote-hostname-or-IP


#### options

The command-line options. See Options.

#### pid

The process ID for which the memory map is to be printed. The process must be a Java process. To get a list of Java processes running on a machine, use the jps(1) command.

#### executable

The Java executable from which the core dump was produced.

#### core

The core file for which the memory map is to be printed.

#### remote-hostname-or-IP

The remote debug server hostname or IP address. See jsadebugd(1).

#### server-id

An optional unique ID to use when multiple debug servers are running on the same remote host.

### Description

The jmap command prints shared object memory maps or heap memory details of a specified process, core file, or remote debug server. If the specified process is running on a 64-bit Java Virtual Machine (JVM), then you might need to specify the -J-d64 option, for example: jmap -J-d64 -heap pid.

> Note: This utility is unsupported and might not be available in future releases of the JDK. On Windows Systems where the dbgeng.dll file is not present, Debugging Tools For Windows must be installed to make these tools work. The PATH environment variable should contain the location of the jvm.dll file that is used by the target process or the location from which the crash dump file was produced, for example: set PATH=%JDK_HOME%\jre\bin\client;%PATH%.

### Options

#### `<no option>`

When no option is used, the jmap command prints shared object mappings. For each shared object loaded in the target JVM, the start address, size of the mapping, and the full path of the shared object file are printed. This behavior is similar to the Oracle Solaris pmap utility.

#### -dump:[live,] format=b, file=filename

Dumps the Java heap in hprof binary format to filename. The live suboption is optional, but when specified, only the active objects in the heap are dumped. To browse the heap dump, you can use the jhat(1) command to read the generated file.

将Java堆以二进制格式hprof转储到文件中。`live` 子选项是可选的,如果指定该选项, 则只会导出堆内存中的存活对象。要查看堆转储, 可以使用 `jhat` 命令来读取生成的文件。

> 示例: `jmap -dump:live,format=b,file=/usr/local/6578_160613.hprof 6578`


#### -finalizerinfo

Prints information about objects that are awaiting finalization.

输出等待终结(awaiting finalization)的对象信息。


> 示例: `jmap -finalizerinfo 6578`

#### -heap

Prints a heap summary of the garbage collection used, the head configuration, and generation-wise heap usage. In addition, the number and size of interned Strings are printed.

输出垃圾收集使用的堆内存汇总信息,包括 head 配置和各个分代(generation-wise)的堆使用情况。此外,也会输出内部化字符串(interned Strings)的数量和大小。

> 示例: `jmap -heap 6578`


#### -histo[:live]

Prints a histogram of the heap. For each Java class, the number of objects, memory size in bytes, and the fully qualified class names are printed. The JVM internal class names are printed with an asterisk (*) prefix. If the live suboption is specified, then only active objects are counted.


输出堆内存的直方图统计信息。会输出每个Java类的实例数量,占用内存大小(字节),以及完全限定类名。JVM内部类名会带有一个星号(*)前缀。如果指定 `live` 子选项, 则只会统计存活对象(JVM会先触发gc，再进行统计)。


> 示例: `jmap -histo:live 6578 > histo_6578.log`



#### -clstats

Prints class loader wise statistics of Java heap. For each class loader, its name, how active it is, address, parent class loader, and the number and size of classes it has loaded are printed.

输出以类加载器为维度识别的Java堆统计信息。对每个类加载器(class loader), 名字,活跃状态,地址, 父加载器, 加载类的数量和大小都会打印出来。

> JDK7 不提供支持.

> 示例: `jmap -clstats 6578`

#### -F

Force. Use this option with the jmap -dump or jmap -histo option when the pid does not respond. The live suboption is not supported in this mode.

强制模式。 在使用 `jmap -dump` or `jmap -histo` 选项时,如果  pid 不响应,则可以使用此参数。 注意: `live`子选项不支持此模式。

#### -h

输出帮助信息(help message).

#### -help

输出帮助信息(help message).

#### -J<flag>


通过此选项,指定用于 **运行jmap的Java虚拟机** 的参数。

> 示例: `jmap -J-Xmx128m 6578`


### 另请参见

- jhat(1)

- jps(1)

- jsadebugd(1)

原文链接:  [http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jmap.html](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jmap.html)


