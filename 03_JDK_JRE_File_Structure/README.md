# 3. JDK and JRE File Structure

# 3. JDK 和 JRE 的目录结构


This chapter introduces the JDK directories and the files they contain. The file structure of the JRE is identical to the structure of the jre directory in the JDK.

本章介绍JDK目录和其中包含的文件。JRE的文件结构与JDK 内部的 jre 目录结构是一致的。


This chapter covers the following topics:

本章包括以下主题:


- Demos and Samples

- 演示和示例



- Development Files and Directories

- 开发包的文件和目录



- Additional Files and Directories

- 附加文件和目录



## Demos and Samples

## 演示和示例


Demos and samples that show you how to program for the Java platform are available as a separate download at the Java SE Downloads page at
http://www.oracle.com/technetwork/java/javase/downloads/index.html

演示和示例,告诉你如何使用Java平台编程, 可以在 Java SE的下载页面单独下载: 
[http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html)


These are available as separate .tar.z compressed packages and .tar.gz compressed binaries. Similar to other 64-bit bundles on Oracle Solaris, the 64-bit demos and samples bundles on Oracle Solaris require that the 32-bit demos and samples bundles to also be installed.

都可作为单独的 `.tar.z` 压缩包或者 `.tar.gz` 压缩包。和其他的64位程序包一样, 在Oracle Solaris上, 64位的 emos and samples 需要安装32位的 demos and samples。


## Development Files and Directories

## 开发包的文件和目录


This section describes the most important files and directories required to develop applications for the Java platform. Some of the directories that are not required include Java source code and C header files. See Additional Files and Directories.

本节描述Java平台开发所需的最重要的文件和目录. 有些目录不是必须的, 例如 Java源代码和C头文件。参加下方的 [附加文件和目录]()。


	jdk1.8.0
	     bin
		  java*
		  javac*
		  javap*
		  javah*
		  javadoc*
	     lib
		  tools.jar
		  dt.jar
	     jre
		  bin
		       java*
		  lib
		       applet
		       ext
			    jfxrt.jar
			    localdata.jar
		       fonts
		       security
		       sparc
			    server
			    client
		       rt.jar
		       charsets.jar




Assuming the JDK software is installed at /jdk1.8.0, here are some of the most important directories:

假设 JDK 安装目录为 `/jdk1.8.0`, 下面是一些最重要的目录:


> ### /jdk1.8.0



Root directory of the JDK software installation. Contains copyright, license, and README files. Also contains src.zip, the archive of source code for the Java platform.

JDK的根目录。包含 copyright, license 和 README文件。还包含Java平台的源代码压缩包 `src.zip`。


> ### /jdk1.8.0/bin



Executables for all the development tools contained in the JDK. The PATH environment variable should contain an entry for this directory.

包含了JDK中所有可执行的开发工具。PATH环境变量应该包含此目录。


> ### /jdk1.8.0/lib



Files used by the development tools. Includes tools.jar, which contains non-core classes for support of the tools and utilities in the JDK. Also includes dt.jar, the DesignTime archive of BeanInfo files that tell interactive development environments (IDEs) how to display the Java components and how to let the developer customize them for an application.

开发者工具(development tools)所使用的文件。包括 `tools.jar`, 其中都是非核心类, 用于支持JDK工具和实用程序。还包括`dt.jar`,  支持 DesignTime 的 BeanInfo文件信息, 告诉交互式开发环境(IDE)如何显示Java组件, 以及如何让开发人员编写应用程序。


> ### /jdk1.8.0/jre




Root directory of the Java Runtime Environment (JRE) used by the JDK development tools. The runtime environment is an implementation of the Java platform. This is the directory referred to by the java.home system property.

JDK development tools所使用的Java运行时环境(JRE)的根目录。运行时环境是Java平台的一个实现, 是系统属性 `java.home` 所指向的目录。


> ### /jdk1.8.0/jre/bin



Executable files for tools and libraries used by the Java platform. The executable files are identical to files in /jdk1.8.0/bin. The java launcher tool serves as an application launcher (and replaced the earlier jre command that shipped with 1.1 releases of the JDK). This directory does not need to be in the PATH environment variable.

Java平台所使用的工具和库的可执行文件。和  `/jdk1.8.0/bin` 下面的是相同的文件。java启动程序工具作为一个应用程序启动器(取代了JDK 1.1版本早期附带的 jre 命令). 此目录不需要在PATH环境变量中指定。


> ### /jdk1.8.0/jre/lib




Code libraries, property settings, and resource files used by the JRE. For example rt.jar contains the bootstrap classes, which are the run time classes that comprise the Java platform core API, and charsets.jar contains the character-conversion classes. Aside from the ext subdirectory, there are several additional resource subdirectories not described here.

JRE使用的代码库、属性设置和资源文件。例如 `rt.jar`, 包含引导类, 是Java平台的运行时类以及核心API。还有 `charsets.jar` 包含字符转换类。 除了 ext 子目录, 其他的几个附加资源子目录这里不再展开描述。


> ### /jdk1.8.0/jre/lib/ext




Default installation directory for extensions to the Java platform. This is where the JavaHelp JAR file goes when it is installed, for example. This directory includes the jfxrt.jar file, which contains the JavaFX runtime libraries and the localedata.jar file, which contains the locale data for the java.text and java.util packages. See The Extension Mechanism at
http://docs.oracle.com/javase/8/docs/technotes/guides/extensions/index.html

Java平台默认的扩展安装目录。这就是 JavaHelp JAR文件安装的位置, 例如。这个目录包括 `jfxrt.jar` 文件, 其中包含了 JavaFX 的运行时库。例如 `localedata.jar` 文件,其中包含了 java.text 和 java.util 包使用的 locale 数据。Java的扩展机制请参考: 
[http://docs.oracle.com/javase/8/docs/technotes/guides/extensions/index.html](http://docs.oracle.com/javase/8/docs/technotes/guides/extensions/index.html)


> ### /jdk1.8.0/jre/lib/security




Contains files used for security management. These include the security policy java.policy and security properties java.security files.

包含用于安全管理的文件。包括 `java.policy` 安全策略，以及 `java.security` 安全属性文件。




> ### /jdk1.8.0/jre/lib/sparc



Contains the .so (shared object) files used by the Oracle Solaris release of the Java platform.

包含了 Java平台Oracle Solaris版本的 `.so` 文件。(.so 是 shared object 的缩写,类似于windows平台的 .dll 文件)。


> ### /jdk1.8.0/jre/lib/sparc/client



Contains the .so file used by the Java HotSpot VM client, which is implemented with Java HotSpot VM technology. This is the default Java Virtual Machine (JVM).

包含了 Java HotSpot VM client 客户端所使用的`.so`文件, 使用Java HotSpot VM技术实现。这是sparc下默认的Java虚拟机(JVM)。


> ### /jdk1.8.0/jre/lib/sparc/server



Contains the .so file used by the Java HotSpot VM server.

包含了 sparc 下 Java HotSpot VM server 使用的 `.so` 文件。


> ### /jdk1.8.0/jre/lib/applet




JAR files that contain support classes for applets can be placed in the lib/applet/ directory. This reduces startup time for large applets by allowing applet classes to be preloaded from the local file system by the applet class loader and provides the same protections as though they had been downloaded over the Internet.

支持 applet 的JAR文件可以放在 `lib/applet/` 目录中. 这降低了大型applet的启动时间,通过允许从本地文件系统预加载applet类,就像他们是从网络上下载的一样,提供了相同的安全保护。

> 说明: Java即将删除Applet.当年一项惊爆眼球的酷炫技术,没能发展起来。 参考: [Oracle即将删除 Applet 插件](http://blog.csdn.net/renfufei/article/details/50609612)

> ### /jdk1.8.0/jre/lib/fonts


Font files used by the platform.

平台使用的字体文件。


## Additional Files and Directories

## 附加文件和目录


This section describes the directory structure for Java source code, C header files, and other additional directories and files.

本节描述 Java source code,和 C header files 的目录结构, 以及其他附加的目录和文件。


	jdk1.8.0
	     db
	     include
	     man
	     src.zip




> ### /jdk1.8.0/src.zip


Archive that contains the source code for the Java platform.

包含Java平台源代码的归档文件(Archive,一般就是压缩包)。


> ### /jdk1.8.0/db


Contains Java DB. See Java DB Technical Documentation at
http://docs.oracle.com/javadb/

包含 Java DB， 详情请参考 Java DB 技术文档:  [http://docs.oracle.com/javadb/](http://docs.oracle.com/javadb/)


> ### /jdk1.8.0/include


C-language header files that support native-code programming with the Java Native Interface and the Java Virtual Machine (JVM) Debugger Interface. See Java Native Interface at
http://docs.oracle.com/javase/8/docs/technotes/guides/jni/index.html

C语言的头文件, 用于支持JNI(Java Native Interface)编程与Java虚拟机(JVM)调试器接口编程。请参考 Java Native Interface 文档:  
[http://docs.oracle.com/javase/8/docs/technotes/guides/jni/index.html](http://docs.oracle.com/javase/8/docs/technotes/guides/jni/index.html)



See also Java Platform Debugger Architecture (JPDA) at
http://docs.oracle.com/javase/8/docs/technotes/guides/jpda/index.html

另请参见 Java平台调试器体系结构文档(JPDA, Java Platform Debugger Architecture):  
[http://docs.oracle.com/javase/8/docs/technotes/guides/jpda/index.html](http://docs.oracle.com/javase/8/docs/technotes/guides/jpda/index.html)


> ### /jdk1.8.0/man



Contains man pages for the JDK tools.

包含JDK tools 的用户手册(man 是 manual 的缩写)。


原文链接: [http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jdkfiles.html](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jdkfiles.html)





