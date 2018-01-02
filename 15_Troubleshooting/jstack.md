## jstack

Prints Java thread stack traces for a Java process, core file, or remote debug server. This command is experimental and unsupported.

打印Java线程栈跟踪信息, 目标可以是 Java进程, core文件,或支持远程调试的服务器。此命令是实验性质的,官方不提供服务支持。


### Synopsis

### 简介

```
jstack [ *options* ] pid

jstack [ *options* ] executable core

jstack [ *options* ] [ *server-id*@ ] remote-hostname-or-IP
```

#### options


The command-line options. See Options.

jstack支持的命令行选项。请参考下面的 [选项] 部分。


#### pid

The process ID for which the stack trace is printed. The process must be a Java process. To get a list of Java processes running on a machine, use the jps(1) command.

进程ID, 要打印线程栈跟踪信息的进程ID，必须是一个Java进程。要获得某台机器上的 Java进程列表, 请使用 `jps` 命令。


#### executable

The Java executable from which the core dump was produced.

Java可执行的核心转储。


#### core

The core file for which the stack trace is to be printed.

核心文件, 要显示堆栈跟踪信息的核心文件。


#### remote-hostname-or-IP

The remote debug server hostname or IP address. See jsadebugd(1).

支持远程调试的服务器的主机名或IP地址。详情请参考 `jsadebugd` 命令。


#### server-id

An optional unique ID to use when multiple debug servers are running on the same remote host.

可选参数, 如果远程主机上同时运行了多个支持远程调试的服务器, 则需要指定唯一ID。


### Description

### 描述


The jstack command prints Java stack traces of Java threads for a specified Java process, core file, or remote debug server. For each Java frame, the full class name, method name, byte code index (BCI), and line number, when available, are printed. With the -m option, the jstack command prints both Java and native frames of all threads with the program counter (PC). For each native frame, the closest native symbol to PC, when available, is printed. C++ mangled names are not demangled. To demangle C++ names, the output of this command can be piped to c++filt. When the specified process is running on a 64-bit Java Virtual Machine, you might need to specify the -J-d64 option, for example: jstack -J-d64 -m pid.

jstack命令打印Java Java线程的堆栈跟踪指定的Java进程,核心文件,或远程调试服务器.对于每一个Java框架,完整的类名,方法名,字节码指数(BCI),和行号,当可用时,打印出来.使用- m选项,jstack命令打印所有线程的Java和本地框架程序计数器(PC).对于每一个本地框架,最接近电脑,本地符号可用时,打印。c++不是demangled支离破碎的名字。demangle c++名称,该命令的输出可以输送到c++ filt.当指定的进程上运行一个64位的Java虚拟机,您可能需要指定-J-d64选项,例如:jstack -J-d64 - m pid。


> **Note**: This utility is unsupported and might not be available in future release of the JDK. In Windows Systems where the dbgeng.dll file is not present, Debugging Tools For Windows must be installed so these tools work. The PATH environment variable needs to contain the location of the jvm.dll that is used by the target process, or the location from which the crash dump file was produced. For example:

> **注意**:这个工具是不支持的,可能不会在未来版本的JDK。dbgeng在Windows系统.Windows dll文件不存在,调试工具必须安装这些工具的工作。PATH环境变量需要包含jvm的位置.使用dll,目标过程,或崩溃转储文件的位置。例如:



	set PATH=<jdk>\jre\bin\client;%PATH%

### Options

### 选项


#### -F

Force a stack dump when jstack [-l] pid does not respond.

如果 `jstack [-l] pid` 不响应, 则强制进行 stack 转储。


#### -l

Long listing. Prints additional information about locks such as a list of owned java.util.concurrent ownable synchronizers. See the AbstractOwnableSynchronizer class description at
http://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/AbstractOwnableSynchronizer.html

打印详细信息(Long listing)。打印关于锁的额外信息, 如持有 `java.util.concurrent` 的锁定者。详情请参考 `AbstractOwnableSynchronizer` 类的描述信息:
[http://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/AbstractOwnableSynchronizer.html](http://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/AbstractOwnableSynchronizer.html)


#### -m

Prints a mixed mode stack trace that has both Java and native C/C++ frames.

打印混合模式(mixed mode)的堆栈跟踪, 包括Java和C/C++框架。


#### -h

Prints a help message.

打印帮助(help)信息。


#### -help

Prints a help message.

打印帮助(help)信息。


### Known Bugs

### 已知的Bug


In mixed mode stack trace, the -m option does not work with the remote debug server.

在混合模式下, `-m` 选项不支持远程调试服务器。


### See Also

### 另请参阅


- jps(1)

- jsadebugd(1)

原文链接: [http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstack.html](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstack.html)

