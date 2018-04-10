# 13.3 jstatd

# 13.3 jstatd

Monitors Java Virtual Machines (JVMs) and enables remote monitoring tools to attach to JVMs. This command is experimental and unsupported.

监控Java虚拟机,使得远程监控工具可以连接到本机的JVM。该命令尚处于实验阶段。 

## Synopsis

## 用法

**jstatd** [ _options_ ]`_options_`

The command-line options. See[Options](#BABGDCJC).

命令行选项参数。请参考下方的 [Options](#) 部分。

## Description

## jstatd简介

The `jstatd` command is an RMI server application that monitors for the creation and termination of instrumented Java HotSpot VMs and provides an interface to enable remote monitoring tools to attach to JVMs that are running on the local host.

`jstatd` 是一款RMI服务器程序, 用来监视 Java HotSpot VM 的启动和关闭, 并为客户端提供接口, 让远程监控工具可以连接到服务器上运行的JVM实例。

The `jstatd` server requires an RMI registry on the local host. The `jstatd` server attempts to attach to the RMI registry on the default port, or on the port you specify with the `-p` `port` option. If an RMI registry is not found, then one is created within the `jstatd` application that is bound to the port that is indicated by the `-p` `port` option or to the default RMI registry port when the `-p` `port` option is omitted. You can stop the creation of an internal RMI registry by specifying the `-nr` option.

`jstatd` 服务器要求本地机器上有一个RMI注册服务(RMI registry)。启动时 `jstatd` 服务器会尝试连接到默认的RMI注册端口, 当然这个端口号也可以通过 `-p port_num` 选项来指定。如果没有找到RMI注册服务, 则会自动创建一个内部的RMI注册服务, 端口号参考 `-p` 参数或默认值。如果不想创建 RMI registry, 可以通过 `-nr` 选项启动。

## Options

## 选项(Options)

* `-nr`
  Does not attempt to create an internal RMI registry within the `jstatd` process when an existing RMI registry is not found.
  
  如果没有找到RMI注册服务, 禁止 `jstatd` 在进程内创建 RMI 注册服务。

* `-p _port_`
  The port number where the RMI registry is expected to be found, or when not found, created if the `-nr` option is not specified.
  
  指定 RMI registry 的端口号, 如果没有找到 RMI registry, 则用该端口号创建(如果指定 `-nr` 选项则不创建)。

* `-n rminame`
  Name to which the remote RMI object is bound in the RMI registry. The default name is `JStatRemoteHost`. If multiple `jstatd` servers are started on the same host, then the name of the exported RMI object for each server can be made unique by specifying this option. However, doing so requires that the unique server name be included in the monitoring client's `hostid` and `vmid` strings.
  
  绑定到RMI注册服务的 remote RMI object 名称。默认名称为 `JStatRemoteHost`。如果需要再同一台主机上启动多个 `jstatd` 服务, 则可以使用此选项来指定唯一名称. 当然, 指定名称后, 客户端连接时需要 `hostid` 和 `vmid` 都对应。

* `-J_option_`
  Passes `option` to the JVM, where option is one of the `options` described on the reference page for the Java application launcher. For example, `-J-Xms48m` sets the startup memory to 48 MB. See[`java`(1)](java.html#CBBFHAJA).
  
  传递给底层 JVM 的 `option`, 这类选项就是JVM启动参数。例如, `-J-Xms48m` 就表示设置启动内存为 `48 MB`。详情请参考 [`java`]() 命令相关页面。

## Security

## 安全问题(Security)

The `jstatd` server can only monitor JVMs for which it has the appropriate native access permissions. Therefore, the `jstatd` process must be running with the same user credentials as the target JVMs. Some user credentials, such as the root user in Solaris, Linux, and OS X operating systems, have permission to access the instrumentation exported by any JVM on the system. A `jstatd` process running with such credentials can monitor any JVM on the system, but introduces additional security concerns.

`jstatd`服务器只能监视当前用户权限范围内的JVM实例。因此, `jstatd` 进程必须和要监控的目标JVM用相同的用户权限启动. 在某些系统, 例如 Solaris,Linux和OS X操作系统中, root 用户有权限访问所有的JVM。所以用 root 权限启动的 `jstatd` 进程可以监视系统中运行的所有JVM, 但这样又容易引入其他安全问题。

The `jstatd` server does not provide any authentication of remote clients. Therefore, running a `jstatd` server process exposes the instrumentation export by all JVMs for which the `jstatd` process has access permissions to any user on the network. This exposure might be undesirable in your environment, and therefore, local security policies should be considered before you start the `jstatd` process, particularly in production environments or on networks that are not secure.

`jstatd`服务器不对远程客户端进行任何身份校验。因此, 启动 `jstatd` 服务器进程就会将所有的jvm暴露给网络上的任何用户。在某些环境中这是不可取的, 因此, 在启动 `jstatd` 进程之前应指定本地安全策略(local security policies), 特别是在生产环境或者不安全的网络环境中(如公网)。

The `jstatd` server installs an instance of `RMISecurityPolicy` when no other security manager is installed, and therefore, requires a security policy file to be specified. The policy file must conform to Default Policy Implementation and Policy File Syntax at

如果没有安装其他的安全管理器, 则`jstatd`服务器会安装一个 `RMISecurityPolicy` 实例, 因此,需要指定一个安全策略文件(security policy file)。策略文件必须符合默认的 Policy 实现规范, 以及 Policy 文件语法格式。 参考: <http://docs.oracle.com/javase/8/docs/technotes/guides/security/PolicyFiles.html>


The following policy file allows the `jstatd` server to run without any security exceptions. This policy is less liberal than granting all permissions to all code bases, but is more liberal than a policy that grants the minimal permissions to run the `jstatd` server.

下面的策略文件, 允许 `jstatd` 服务器不抛出任何安全性异常. 该策略比授予所有代码权限要严格, 却又比最小权限要宽松很多。

```
grant codebase "file:${java.home}/../lib/tools.jar" {   
    permission java.security.AllPermission;
};

```



To use this policy setting, copy the text into a file called `jstatd.all.policy` and run the `jstatd` server as follows:

要使用该策略, 将其中的文本粘贴到文件 `jstatd.all.policy` 之中, 并在启动 `jstatd`服务时指定:

```
jstatd -J-Djava.security.policy=/xxxPath/jstatd.all.policy

```


For sites with more restrictive security practices, it is possible to use a custom policy file to limit access to specific trusted hosts or networks, though such techniques are subject to IP address spoofing attacks. If your security concerns cannot be addressed with a customized policy file, then the safest action is to not run the `jstatd` server and use the `jstat` and `jps` tools locally.

对于安全措施更加严格的网站, 可以自定义策略文件, 用以限制客户端只能是特定的受信主机或这网段, 尽管这些技术可能被IP地址欺骗所攻击。如果不能通过定制的策略文件解决安全问题, 那么最好还是不要启动 `jstatd` 服务, 而是在服务器本地使用 `jstat` 和 `jps` 工具。

## Remote Interface

## 远程接口

The interface exported by the `jstatd` process is proprietary and guaranteed to change. Users and developers are discouraged from writing to this interface.

`jstatd` 进程所提供的接口是专用的, 随时可能发生变化, 不建议用户和开发者操作这些接口。

## Examples

## 示例

The following are examples of the `jstatd` command. The `jstatd` scripts automatically start the server in the background

下面是 `jstatd` 命令的示例。这些脚本会在后台自动启动 `jstatd` 服务。

### Internal RMI Registry

### 内部RMI注册服务

This example shows how to start a `jstatd` session with an internal RMI registry. This example assumes that no other server is bound to the default RMI registry port (port 1099).

以下示例展示了如何启动 `jstatd` 以及内部RMI注册服务. 假定没有其他服务绑定到默认的RMI注册端口 (1099)。

```
jstatd -J-Djava.security.policy=all.policy

```



### External RMI Registry

### 外部RMI注册服务

This example starts a `jstatd` session with a external RMI registry.

以下示例演示如何启动`jstatd`会话, 以及外部RMI注册服务。

```
rmiregistry &
jstatd -J-Djava.security.policy=all.policy
```



This example starts a `jstatd` session with an external RMI registry server on port 2020.


以下示例演示如何启动`jstatd`会话, 以及在端口2020上启动外部RMI注册服务。

```
jrmiregistry 2020&amp;
jstatd -J-Djava.security.policy=all.policy -p 2020

```



This example starts a `jstatd` session with an external RMI registry on port 2020 that is bound to `AlternateJstatdServerName`.


以下示例演示如何启动`jstatd`会话, 以及在端口2020上启动外部RMI注册服务, 并且指定了 remote RMI object 的名称为 `AlternateJstatdServerName`。

```
rmiregistry 2020&amp;
jstatd -J-Djava.security.policy=all.policy -p 2020
    -n AlternateJstatdServerName

```



### Stop the Creation of an In-Process RMI Registry

### 禁止创建进程内置的RMI注册服务

This example starts a `jstatd` session that does not create an RMI registry when one is not found. This example assumes an RMI registry is already running. If an RMI registry is not running, then an error message is displayed.

以下示例启动 `jstatd` 服务, 如果没有现成的RMI注册服务, 也不准在内部创建。假定已经有 RMI 注册服务在运行中。 如果没有 RMI注册服务, 则启动失败, 并显示错误消息。

```
jstatd -J-Djava.security.policy=all.policy -nr

```



### Enable RMI Logging

### 启用 RMI 日志记录

This example starts a `jstatd` session with RMI logging capabilities enabled. This technique is useful as a troubleshooting aid or for monitoring server activities.

以下示例启动 `jstatd` 服务, 并启用 RMI 日志记录功能。日志功能对于监控服务器行为, 以及故障排除来说是很有用的。

```
jstatd -J-Djava.security.policy=all.policy
    -J-Djava.rmi.server.logCalls=true

```



## See Also

## 另请参阅

*   [`java`(1)](java.html#CBBFHAJA)

*(`java`(1)](CBBFHAJA)java.html #

*   [`jps`(1)](jps.html#CHDGHCGB)

*(`jps`(1)](CHDGHCGB)jps.html #

*   [`jstat`(1)](jstat.html#BEHBBBDJ)

*(`jstat`(1)](BEHBBBDJ)jstat.html #

*   [`rmiregistry`(1)](rmiregistry.html#CHDEDDIE)

*(`rmiregistry`(1)](CHDEDDIE)rmiregistry.html #

原文链接: <https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstatd.html>



