## jhat

## JDK内置故障排查工具: jhat 简介


Analyzes the Java heap. This command is experimental and unsupported.

Java Heap(dump文件)分析工具; 此工具为实验性质、官方不提供技术支持。


### Synopsis

### 用法

```
jhat [ options ] heap-dump-file
```

- *heap-dump-file*

  Java binary heap dump file to be browsed. For a dump file that contains multiple heap dumps, you can specify which dump in the file by appending `#<number>` to the file name, for example, `myfile.hprof#3`.

  要查看的二进制Java堆转储文件(Java binary heap dump file)。 如果某个转储文件中包含了多份 heap dumps, 可在文件名之后以 `#<number>` 的方式指定解析哪一个 dump, 如: `myfile.hprof#3`


### 示例 ##

使用jmap工具转储堆内存、可以使用如下方式: 

```
jmap -dump:file=DumpFileName.txt,format=b <pid>
```
	
例如: 

```
jmap -dump:file=D:/javaDump.hprof,format=b 3614
Dumping heap to D:\javaDump.hprof ...
Heap dump file created
```

其中, 3614 是java进程的ID,一般来说, jmap 需要和目标JVM的版本一致或者兼容,才能成功导出. 如果不知道如何使用,直接输入 `jmap`, 或者 `jmap -h` 可看到提示信息.

然后分析时使用jhat命令,如下所示:

```
jhat -J-Xmx1024m D:/javaDump.hprof
...... 其他信息 ...
Snapshot resolved.
Started HTTP server on port 7000
Server is ready.
```

使用参数 `-J-Xmx1024m` 是因为默认JVM的堆内存可能不足以加载整个dump 文件. 可根据需要进行调整. 根据提示知道端口号是 7000,

接着使用浏览器访问 <http://localhost:7000/> 即可看到相关信息.


### Description

### 详细说明

The `jhat` command parses a Java heap dump file and starts a web server. The `jhat` command lets you to browse heap dumps with your favorite web browser. The `jhat` command supports predesigned queries such as show all instances of a known class `MyClass`, and Object Query Language (OQL). OQL is similar to SQL, except for querying heap dumps. Help on OQL is available from the OQL help page shown by the `jhat` command. With the default port, OQL help is available at http://localhost:7000/oqlhelp/

`jhat`命令解析堆转储文件, 并启动 web server. 然后可以通过浏览器来查看/浏览 dump 出来的 heap.  `jhat` 命令支持预定义的查询, 例如显示某个类的所有实例. 还支持 **对象查询语言**(OQL, Object Query Language)。 OQL有点类似SQL,专门用来查询堆转储。 OQL相关的帮助信息可以在 jhat 命令所提供的服务器页面最底部. 如果使用默认端口, 则OQL帮助信息页面为: <http://localhost:7000/oqlhelp/>


There are several ways to generate a Java heap dump:

生成JVM堆转储文件(heap dump)的方式有多种:

- Use the `jmap -dump` option to obtain a heap dump at runtime. See [`jmap`(1)](./jmap.md)

- 使用 `jmap  -dump` 选项获取JVM运行时的堆转储. (可以参考上面的示例)详情参见: [`jmap`(1)](./jmap.md)

- Use the `jconsole` option to obtain a heap dump through `HotSpotDiagnosticMXBean` at runtime. See [`jconsole`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jconsole.html) and the `HotSpotDiagnosticMXBean` interface description at `http://docs.oracle.com/javase/8/docs/jre/api/management/extension/com/sun/management/HotSpotDiagnosticMXBean.html`

- 使用 `jconsole` 选项通过 HotSpotDiagnosticMXBean 从运行时获得堆转储。 请参考: 
  * [jconsole(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jconsole.html)
  * [`HotSpotDiagnosticMXBean` 接口](http://docs.oracle.com/javase/8/docs/jre/api/management/extension/com/sun/management/HotSpotDiagnosticMXBean.html).

- Heap dump is generated when an `OutOfMemoryError` is thrown by specifying the `-XX:+HeapDumpOnOutOfMemoryError` Java Virtual Machine (JVM) option.

- 如果在JVM启动时指定了 `-XX:+HeapDumpOnOutOfMemoryError` 选项, 则发生 **OutOfMemoryError** 时, 会自动生成堆转储。

- Use the `hprof` command. See the HPROF: A Heap/CPU Profiling Tool at `http://docs.oracle.com/javase/8/docs/technotes/samples/hprof.html`

- 使用 `hprof` 命令。 请参考 性能分析工具-HPROF简介, <https://github.com/cncounter/translation/blob/master/tiemao_2017/20_hprof/20_hprof.md>



### Options

### 选项参数

- `-stack false|true`

  Turns off tracking object allocation call stack. If allocation site information is not available in the heap dump, then you have to set this flag to `false`. The default is `true`.

  关闭对象分配调用栈跟踪(tracking object allocation call stack)。 如果在堆转储中分配位置信息不可用. 则必须将此标志设置为 false. 默认值为 `true`.

- `-refs false|true`

  Turns off tracking of references to objects. Default is `true`. By default, back pointers, which are objects that point to a specified object such as referrers or incoming references, are calculated for all objects in the heap.

  关闭对象引用跟踪(tracking of references to objects)。 默认值为 `true`. 默认情况下, 会统计整个堆内存中所有对象的反向指针, 即指向某个特定对象的对象, 如反向链接(referrers)或指向对象的引用(incoming references), 。

- `-port <port-number>`

  Sets the port for the `jhat` HTTP server. Default is 7000.

  设置 jhat HTTP server 的端口号. 默认值 `7000`.

- `-exclude <exclude-file>`

  Specifies a file that lists data members that should be excluded from the reachable objects query. For example, if the file lists `java.lang.String.value`, then, then whenever the list of objects that are reachable from a specific object `o` are calculated, reference paths that involve `java.lang.String.value` field are not considered.

  指定在可达对象查询时，需要排除的文件。 例如, 如果文件列出了 `java.lang.String.value`, 那么当从某个特定对象 `Object o` 计算可达的对象列表时, 涉及 `java.lang.String.value` 的引用路径都会被排除。

- `-baseline <exclude-file>`

  Specifies a baseline heap dump. Objects in both heap dumps with the same object ID are marked as not being new. Other objects are marked as new. This is useful for comparing two different heap dumps.

  指定一个基准堆转储(baseline heap dump)。 在两个 heap dumps 中有相同 object ID 的对象不会被标记为新对象. 其他对象被标记为新对象. 在比较两个堆转储文件时使用.

- `-debug <int>`

  Sets the debug level for this tool. A level of 0 means no debug output. Set higher values for more verbose modes.

  设置 debug 级别。 level `0` 表示不输出调试信息。 值越大则输出更详细的 debug 信息.

- `-version`

  Reports the release number and exits
  
  JVM工具通用选项; 启动后显示版本信息并退出

- `-h`

  Displays a help message and exits.

  JVM工具通用选项; 启动后显示帮助信息并退出. 等同于 `-help`

- `-help`

  Displays a help message and exits.

  JVM工具通用选项; 启动后显示帮助信息并退出. 等同于 `-h`

- `-J<flag>`

  Passes `flag` to the Java Virtual Machine on which the `jhat` command is running. For example, `-J-Xmx512m` to use a maximum heap size of 512 MB.
  
  指定传给底层JVM的参数。 因为 jhat 命令实际上是通过JVM来执行的, 通过 `-J<flag>` 可以给底层JVM传入启动参数`<flag>`. 例如, `-J-Xmx512m` 则指定运行底层JVM可以使用的堆内存最大值为 512 MB. 如果需要传入多个JVM启动参数,则传入多个 `-J<flag>`.





### See Also

### 另请参阅:

- [`jmap`](./jmap.md)
- [`jconsole`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jconsole.html#CACDDJCH)
- 性能分析工具-HPROF简介, <https://github.com/cncounter/translation/blob/master/tiemao_2017/20_hprof/20_hprof.md>

原文链接:  <https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jhat.html>


