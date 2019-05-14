## jcmd

## JDK内置故障排查工具: jcmd 简介

Sends diagnostic command requests to a running Java Virtual Machine (JVM).

将诊断命令发送给正在运行中的Java虚拟机(JVM)。


### Synopsis

### 用法(Synopsis)

```
jcmd [-l|-h|-help]

jcmd pid|main-class PerfCounter.print

jcmd pid|main-class -f filename

jcmd pid|main-class <command [arguments]>
```


### Description

### 详细说明

The `jcmd` utility is used to send diagnostic command requests to the JVM. It must be used on the same machine on which the JVM is running, and have the same effective user and group identifiers that were used to launch the JVM.

`jcmd`工具用于将诊断命令发送到JVM。 必须与目标JVM的同一台机器上才能使用， 并且和JVM必须是相同的启动用户，相同的用户组。


> **Note:** To invoke diagnostic commands from a remote machine or with different identifiers, you can use the `com.sun.management.DiagnosticCommandMBean` interface. For more information about the `DiagnosticCommandMBean` interface, see the API documentation at `http://docs.oracle.com/javase/8/docs/jre/api/management/extension/com/sun/management/DiagnosticCommandMBean.html`

> **注意：** 如果要从远程机器或者不同的用户组来执行诊断命令，可以使用 `com.sun.management.DiagnosticCommandMBean` 接口。 更多关于 `DiagnosticCommandMBean`接口的信息，请参阅API文档： <http://docs.oracle.com/javase/8/docs/jre/api/management/extension/com/sun/management/DiagnosticCommandMBean.html>。

If you run `jcmd` without arguments or with the `-l` option, it prints the list of running Java process identifiers with the main class and command-line arguments that were used to launch the process. Running `jcmd` with the `-h` or `-help` option prints the tool's help message.

如果 `jcmd` 不带参数或者是 `-l` 选项，则会打印出正在运行的Java进程信息, 包括进程标识(pid), main class, 以及命令行启动参数。 使用 `-h` 或`-help` 选项则会打印出帮助消息。

> **Note:** The `jcmd` utility can be used to dynamically interact with Java Flight Recorder (JFR) in a JVM that is already running. You can use it to unlock commercial features, enable/start/stop flight recordings, and obtain various status messages from the system. For a list of examples, see the Java Flight Recorder Runtime Guide at `http://docs.oracle.com/javacomponents/jmc.htm`

> **注意：** `jcmd` 工具可以和JVM中的Java Flight Recorder（JFR）动态交互。 可以用它来解锁商业功能，enable/start/stop 飞行记录仪， 从系统获中取各种状态消息。 有关示例请参阅Java Flight Recorder 运行时指南: <http://docs.oracle.com/javacomponents/jmc.htm>。

If you specify the processes identifier (*pid*) or the main class (*main-class*) as the first argument, `jcmd` sends the diagnostic command request to the Java process with the specified identifier or to all Java processes with the specified name of the main class. You can also send the diagnostic command request to all available Java processes by specifying `0` as the process identifier. Use one of the following as the diagnostic command request:

如果第一个参数是进程标识符（`pid`）或者主类（`main-class`）， `jcmd`将诊断命令发送给对应pid的Java进程，或者发送给对应main-class的所有Java进程。 还可以将进程标识符指定为 `0`， 把诊断命令发送给所有可见的Java进程。 每个诊断命令可以是下面列表中的一项：

- `Perfcounter.print`

  Prints the performance counters available for the specified Java process. The list of performance counters might vary with the Java process.
  
  打印指定Java进程的性能计数器。 每个Java进程的性能计数器列表可能都不一样。

示例:

```
sudo jcmd 12560 PerfCounter.print

12560:
java.property.java.version="1.8.0_74"
sun.gc.cause="No GC"
sun.gc.tlab.alloc=66264539
sun.gc.tlab.allocThreads=53
sun.gc.tlab.gcWaste=574925
sun.gc.tlab.maxGcWaste=56861
sun.gc.tlab.maxSlowAlloc=169
sun.gc.tlab.maxSlowWaste=1987
sun.rt.createVmBeginTime=1557804656707
sun.rt.createVmEndTime=1557804657789
sun.rt.javaCommand="org.apache.catalina.startup.Bootstrap start"
sun.rt.vmInitDoneTime=1557804656802
......
```

- `-f <filename>`

  The name of the file from which to read diagnostic commands and send them to the specified Java process. Used only with the `-f` option. Each command in the file must be written on a single line. Lines starting with a number sign (`#`) are ignored. Processing of the file ends when all lines have been read or when a line containing the `stop` keyword is read.

  从文件中读取诊断命令并将其发送到指定的Java进程。 仅当第一个参数是进程标识符或主类时，才能使用此选项。 
  对应文件中的每个命令都必须写在单独的一行。 
  以井号（`＃`）开头的行是注释，将被忽略。 
  当所有行读取完毕、或者某一行包含`stop`关键字时，文件处理就结束。

- `<command [arguments]>`

  The command to be sent to the specified Java process. The list of available diagnostic commands for a given process can be obtained by sending the `help` command to this process. Each diagnostic command has its own set of arguments. To see the description, syntax, and a list of available arguments for a command, use the name of the command as the argument for the `help` command.
  
  **Note:** If any arguments contain spaces, you must surround them with single or double quotation marks (`'` or `"`). In addition, you must escape single or double quotation marks with a backslash (`\`) to prevent the operating system shell from processing quotation marks. Alternatively, you can surround these arguments with single quotation marks and then with double quotation marks (or with double quotation marks and then with single quotation marks).

  要发送到指定Java进程的命令。 
  可以向此进程发送 `help` 命令来获取可用的诊断命令列表。 
  每个诊断命令都有自己的参数集。 
  要查看某个命令的具体信息和用法，请使用 `help XXX命令` 的方式来查看。

  **注意：** 如果某个参数包含空格，则必须用单引号或双引号(`'` or `"`)引起来。 此外，还必须使用反斜杠（`\`）将单引号和双引号转义，避免操作系统shell将引号给吃掉。 或者，也可以将参数用单引号引起来， 再在外面用双引号引起来（也可以里面用双引号，外层用单引号，shell只吃掉一层）。

示例:

```
# sudo jcmd 12560 help VM.flags
# sudo jcmd 12560 VM.flags
sudo jcmd 12560 help

12560:
The following commands are available:
JFR.stop
JFR.start
JFR.dump
JFR.check
VM.native_memory
VM.check_commercial_features
VM.unlock_commercial_features
ManagementAgent.stop
ManagementAgent.start_local
ManagementAgent.start
GC.rotate_log
Thread.print
GC.class_stats
GC.class_histogram
GC.heap_dump
GC.run_finalization
GC.run
VM.uptime
VM.flags
VM.system_properties
VM.command_line
VM.version
help

For more information about a specific command use 'help <command>'.
```

### Options

### 选项(Options)

Options are mutually exclusive.

各个选项是互斥的。

- `-f <filename>`

  Reads commands from the specified file. This option can be used only if you specify the process identifier or the main class as the first argument. Each command in the file must be written on a single line. Lines starting with a number sign (`#`) are ignored. Processing of the file ends when all lines have been read or when a line containing the `stop` keyword is read.

  从指定文件中读取命令。 仅当第一个参数是进程标识符或主类时，才能使用此选项。 
  对应文件中的每个命令都必须写在单独的一行。 
  以井号（`＃`）开头的行是注释，将被忽略。 
  当所有行读取完毕、或者某一行包含`stop`关键字时，文件处理就结束。


- `-h`

- `-help`

  Prints a help message.

  显示帮助信息.

示例:

```
# jcmd -help
jcmd -h
Usage: jcmd <pid | main class> <command ...|PerfCounter.print|-f file>
   or: jcmd -l                                                    
   or: jcmd -h                                                    
                                                                  
  command must be a valid jcmd command for the selected jvm.      
  Use the command "help" to see which commands are available.   
  If the pid is 0, commands will be sent to all Java processes.   
  The main class argument will be used to match (either partially 
  or fully) the class used to start Java.                         
  If no options are given, lists Java processes (same as -p).     
                                                                  
  PerfCounter.print display the counters exposed by this process  
  -f  read and execute commands from the file                     
  -l  list JVM processes on the local machine                     
  -h  this help  
```

- `-l`

  Prints the list of running Java processes identifiers with the main class and command-line arguments.

  打印出正在运行的Java进程信息, 包括进程标识(pid), main class, 以及命令行启动参数。

示例:

```
# sudo jcmd -l | grep -v jcmd
sudo jcmd -l

2760 org.apache.catalina.startup.Bootstrap start
7689 sun.tools.jcmd.JCmd -l
1083 org.apache.catalina.startup.Bootstrap start
```


### See Also

### 另请参见

- [jps](../13_Monitor_JVM/1301_jps.md)

