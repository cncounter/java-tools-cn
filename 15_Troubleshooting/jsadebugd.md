## jsadebugd

Attaches to a Java process or core file and acts as a debug server. This command is experimental and unsupported.


### Synopsis

```
jsadebugd pid [server-id]

jsadebugd executable core [server-id]
```

- `pid`

  The process ID of the process to which the debug server attaches. The process must be a Java process. To get a list of Java processes running on a machine, use the [`jps`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jps.html) command. At most one instance of the debug server can be attached to a single process.

- `executable`

  The Java executable from which the core dump was produced.

- `core`

  The core file to which the debug server should attach.

- `server-id`

  An optional unique ID that is needed when multiple debug servers are started on the same machine. This ID must be used by remote clients to identify the particular debug server to which to attach. Within a single machine, this ID must be unique.

### Description

The `jsadebugd` command attaches to a Java process or core file and acts as a debug server. Remote clients such as `jstack`, `jmap`, and `jinfo` can attach to the server through Java Remote Method Invocation (RMI). Before you start the `jsadebugd` command, start the RMI registry with the `rmiregistry` command as follows where `$JAVA_HOME` is the JDK installation directory:

```
rmiregistry -J-Xbootclasspath/p:$JAVA_HOME/lib/sajdi.jar
```

If the RMI registry was not started, then the `jsadebugd` command starts an RMI registry in a standard (1099) port internally. The debug server can be stopped by sending a `SIGINT` to it. To send a SIGINT press **Ctrl+C**.

**Note:** This utility is unsupported and may or may not be available in future releases of the JDK. In Windows Systems where `dbgeng.dll` is not present, Debugging Tools For Windows must be installed to have these tools working. The `PATH` environment variable should contain the location of jvm.dll used by the target process or the location from which the crash dump file was produced. For example, `set PATH=%JDK_HOME%\jre\bin\client;%PATH%`.





### See Also


- [`jinfo`(1)](./jinfo.md)
- [`jmap`(1)](./jmap.md)
- [`jps`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jps.html)
- [`jstack`(1)](./jstack.html)
- [`rmiregistry`(1)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/rmiregistry.html)

