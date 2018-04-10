# 13.3 jstatd

监控本地JVM, 允许远程监控工具连接到本机的JVM。该命令尚处于实验阶段，官方不提供技术支持。 


## 用法

**jstatd** [ _options_ ]`_options_`

命令行选项参数。请参考下方的 [Options](#) 部分。

## jstatd简介

`jstatd` 是一个RMI服务器程序, 用来监控 Java HotSpot VM 的启动和关闭, 并提供接口, 允许远程监控客户端连接服务器本地运行的JVM实例。


`jstatd` 服务端需要本机存在 RMI注册服务(RMI registry)。在 `jstatd` 服务启动时,会尝试连接默认的RMI注册端口, 当然, 也可以通过 `-p port_num` 选项来指定这个端口号。如果没有找到RMI注册服务, 则会自动创建一个内置的 RMI注册服务, 端口号通过 `-p` 参数指定, 不指定则使默认值。如果不想创建 RMI注册服务, 可以通过 `-nr` 选项启动。


## 选项(Options)

* `-nr`
  如果没有找到RMI注册服务, 禁止 `jstatd` 在进程内创建RMI注册服务。

* `-p _port_`
  指定该端口号来查询RMI注册服务, 如果没有找到, 则默认会使用该端口号来创建(指定 `-nr` 选项则不创建)。

* `-n rminame`  
  绑定到RMI注册服务的 remote RMI object 名称。默认名称为 `JStatRemoteHost`。如果需要在同一台机器上启动多个 `jstatd` 服务, 则需要通过该选项来指定唯一名称. 当然, 指定名称后, 客户端连接时需要对应的 `hostid` 和 `vmid`。

* `-J_option_`
  传递给底层JVM的 `option`, 这类选项就是JVM启动参数。例如, `-J-Xms48m` 就表示设置启动内存为 `48MB`。详情请参考 [`java`]() 命令。


## 安全问题(Security)

`jstatd`服务器,只能监控启动该服务的用户权限范围内的JVM实例。因此, `jstatd` 进程必须和要监控的目标JVM使用相同的用户权限启动. 在某些系统, 例如 Solaris,Linux和OS X操作系统中, root 用户可以访问所有的JVM。所以用 root 权限启动 `jstatd` 则可以监控系统上运行的所有JVM, 但这样又容易引起一些安全问题。

`jstatd`服务器对远程客户端不进行任何身份校验。因此, 启动 `jstatd` 服务器会将所有的jvm暴露给网络上的任意用户。在某些环境中这是不可取的, 因此, 在启动 `jstatd` 进程之前应指定本地安全策略(local security policies), 特别是在生产环境或者不安全的网络环境中(如公网)。

如果没有安装其他的安全管理器, 则`jstatd`服务器会安装一个 `RMISecurityPolicy` 实例, 因此,需要指定一个安全策略文件(security policy file)。策略文件必须符合默认的 Policy 实现规范, 以及 Policy 文件语法格式。 参考: <http://docs.oracle.com/javase/8/docs/technotes/guides/security/PolicyFiles.html>


下面的策略文件, 允许 `jstatd` 服务器不抛出任何安全性异常. 该策略比授予所有代码权限要严格, 却又比最小权限要宽松很多。

```
grant codebase "file:${java.home}/../lib/tools.jar" {   
    permission java.security.AllPermission;
};

```


要使用这种策略, 将其中的文本粘贴到文件 `jstatd.all.policy` 之中, 并在启动 `jstatd` 时指定如下选项:

```
jstatd -J-Djava.security.policy=/xxxPath/jstatd.all.policy

```


需要更强安全措施的网站, 可以自定义策略文件, 用以限制客户端只能是特定的受信主机或某些网段, 尽管这些技术可能受到IP地址欺骗技术的攻击。如果不能通过定制的策略文件解决安全问题, 那么最好不要启动 `jstatd` 服务, 只能在服务器本地使用 `jstat` 和 `jps` 工具。


## 远程接口

`jstatd` 服务提供的接口是专用的, 随时可能发生变化, 不建议用户和开发者操作这些接口。


## 示例

下面是 `jstatd` 命令的使用示例。这些脚本会自动在后台启动 `jstatd` 服务。


### 内置的RMI注册服务

以下示例展示了如何启动 `jstatd` 以及内置的RMI注册服务. 这里假定没有其他服务绑定到默认的RMI注册端口 (默认 `1099`)。

```
jstatd -J-Djava.security.policy=all.policy

```

### 外部RMI注册服务

以下示例演示如何使用外部RMI注册服务来启动 `jstatd`。

```
rmiregistry &
jstatd -J-Djava.security.policy=all.policy
```

> rmiregistry 监听了 1099端口, jstatd尝试连接该端口, 成功则不会创建内置的RMI注册服务。

以下示例, 展示了在端口2020上启动外部RMI注册服务, 并启动`jstatd`。

```
jrmiregistry 2020 &
jstatd -J-Djava.security.policy=all.policy -p 2020

```

> rmiregistry 监听了 2020 端口, jstatd尝试连接 2020 端口, 成功则不会创建内置的RMI注册服务。


以下示例, 展示了在端口2020上启动外部RMI注册服务, 并启动`jstatd`, 同时指定了 remote RMI object 的名称为 `AlternateJstatdServerName`。

```
rmiregistry 2020&amp;
jstatd -J-Djava.security.policy=all.policy -p 2020
    -n AlternateJstatdServerName

```


### 禁止创建进程内置的RMI注册服务

以下示例启动 `jstatd` 服务时,禁止创建进程内置的RMI注册服务。 即使没有现成的RMI注册服务, 也不准自动创建。 如果没有 RMI注册服务, 则启动失败, 并显示错误信息。

```
jstatd -J-Djava.security.policy=all.policy -nr

```


### 启用 RMI 日志记录


以下示例启动 `jstatd` 服务, 并启用 RMI日志记录功能。日志功能对于监控服务器行为, 以及故障排除来说非常有用。

```
jstatd -J-Djava.security.policy=all.policy
    -J-Djava.rmi.server.logCalls=true

```


## 另请参阅

*   [`java`(1)](java.html#CBBFHAJA)
*   [`jps`](1301_jps.md)
*   [`jstat`](1302_jstat.md)
*   [`rmiregistry`(1)](rmiregistry.html#CHDEDDIE)


原文链接: <https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstatd.html>



