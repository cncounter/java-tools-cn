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

演示和示例,展示如何计划为Java平台可以作为一个单独的Java SE下载页面下载
http://www.oracle.com/technetwork/java/javase/downloads/index.html


These are available as separate .tar.z compressed packages and .tar.gz compressed binaries. Similar to other 64-bit bundles on Oracle Solaris, the 64-bit demos and samples bundles on Oracle Solaris require that the 32-bit demos and samples bundles to also be installed.

这些可作为单独的. tar。z和. tar压缩包。gz压缩二进制文件.类似于其他64位包在Oracle Solaris,64位演示和样品包在Oracle Solaris要求32位演示和样品包也被安装。


## Development Files and Directories

## 开发包的文件和目录


This section describes the most important files and directories required to develop applications for the Java platform. Some of the directories that are not required include Java source code and C header files. See Additional Files and Directories.

本节描述所需的最重要的文件和目录为Java平台开发应用程序.一些不需要的目录包括Java源代码和C头文件。看到额外的文件和目录。


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

假设软件是安装在/ jdk1.8.0 JDK,下面是一些最重要的目录:


> /jdk1.8.0




Root directory of the JDK software installation. Contains copyright, license, and README files. Also contains src.zip, the archive of source code for the Java platform.

The Root directory of the JDK software installation. The Contains copyright and license, and the README files. Also Contains the SRC. Zip, the archive of the source code for the Java platform.


> /jdk1.8.0/bin




Executables for all the development tools contained in the JDK. The PATH environment variable should contain an entry for this directory.

JDK中包含可执行程序的开发工具。PATH环境变量应该包含这个目录的条目。


> /jdk1.8.0/lib



Files used by the development tools. Includes tools.jar, which contains non-core classes for support of the tools and utilities in the JDK. Also includes dt.jar, the DesignTime archive of BeanInfo files that tell interactive development environments (IDEs) how to display the Java components and how to let the developer customize them for an application.

文件所使用的开发工具。包括工具。jar,它包含非核心类支持JDK工具和实用程序。还包括dt.jar DesignTime BeanInfo文件的存档,告诉交互式开发环境(ide)如何显示Java组件,以及如何让开发人员自定义应用程序。


> /jdk1.8.0/jre




Root directory of the Java Runtime Environment (JRE) used by the JDK development tools. The runtime environment is an implementation of the Java platform. This is the directory referred to by the java.home system property.

根目录的Java运行时环境(JRE)JDK所使用的开发工具。运行时环境是Java平台的一个实现.这是指由java的目录。系统属性。


> /jdk1.8.0/jre/bin




Executable files for tools and libraries used by the Java platform. The executable files are identical to files in /jdk1.8.0/bin. The java launcher tool serves as an application launcher (and replaced the earlier jre command that shipped with 1.1 releases of the JDK). This directory does not need to be in the PATH environment variable.

可执行文件的Java平台所使用的工具和库。可执行文件是相同的文件/ jdk1.8.0 / bin.java启动程序工具作为一个应用程序启动器(取代了早期jre 1.1版本的JDK附带的命令).这个目录不需要在PATH环境变量中。


> /jdk1.8.0/jre/lib




Code libraries, property settings, and resource files used by the JRE. For example rt.jar contains the bootstrap classes, which are the run time classes that comprise the Java platform core API, and charsets.jar contains the character-conversion classes. Aside from the ext subdirectory, there are several additional resource subdirectories not described here.

代码库、属性设置和资源文件使用的JRE。例如rt.jar包含引导类,包括Java平台的运行时类核心API,和数据集。jar包含字符转换类.除了ext目录,有几个额外的资源目录不是这里描述。


> /jdk1.8.0/jre/lib/ext




Default installation directory for extensions to the Java platform. This is where the JavaHelp JAR file goes when it is installed, for example. This directory includes the jfxrt.jar file, which contains the JavaFX runtime libraries and the localedata.jar file, which contains the locale data for the java.text and java.util packages. See The Extension Mechanism at
http://docs.oracle.com/javase/8/docs/technotes/guides/extensions/index.html

扩展Java平台的默认安装目录。这就是JavaHelp JAR文件安装时,例如。这个目录包括jfxrt.jar文件,其中包含和localedata JavaFX运行时库。jar文件,其中包含java语言环境数据。文本和java。util包。看到的扩展机制
http://docs.oracle.com/javase/8/docs/technotes/guides/extensions/index.html


> /jdk1.8.0/jre/lib/security




Contains files used for security management. These include the security policy java.policy and security properties java.security files.




> /jdk1.8.0/jre/lib/sparc




Contains the .so (shared object) files used by the Oracle Solaris release of the Java platform.

包含了。所以(共享对象)文件使用的Oracle Solaris版本的Java平台。


> /jdk1.8.0/jre/lib/sparc/client




Contains the .so file used by the Java HotSpot VM client, which is implemented with Java HotSpot VM technology. This is the default Java Virtual Machine (JVM).

包含了。所以Java HotSpot虚拟机客户端所使用的文件,这是与Java HotSpot VM技术的实现。这是默认的Java虚拟机(JVM)。


> /jdk1.8.0/jre/lib/sparc/server




Contains the .so file used by the Java HotSpot VM server.

包含了。所以Java HotSpot VM服务器所使用的文件。


> /jdk1.8.0/jre/lib/applet




JAR files that contain support classes for applets can be placed in the lib/applet/ directory. This reduces startup time for large applets by allowing applet classes to be preloaded from the local file system by the applet class loader and provides the same protections as though they had been downloaded over the Internet.

为applet JAR文件包含支持类可以放在lib / applet /目录中.这降低了大型applet启动时间允许applet类加载applet从本地文件系统的类装入器,好像他们已经提供了相同的保护 互联网downloaded寺院。


> /jdk1.8.0/jre/lib/fonts




Font files used by the platform.

字体文件使用的平台。


## Additional Files and Directories

## 附加文件和目录


This section describes the directory structure for Java source code, C header files, and other additional directories and files.

本节描述Java源代码的目录结构,C头文件和其他额外的目录和文件。


	jdk1.8.0
	     db
	     include
	     man
	     src.zip




> /jdk1.8.0/src.zip




Archive that contains the source code for the Java platform.

的档案文件,包含Java平台的源代码。


> /jdk1.8.0/db




Contains Java DB. See Java DB Technical Documentation at
http://docs.oracle.com/javadb/

巴林欢迎爪哇分贝。教育部的技术文件at爪哇分贝
http://docs.oracle.com/javadb/


> /jdk1.8.0/include


C-language header files that support native-code programming with the Java Native Interface and the Java Virtual Machine (JVM) Debugger Interface. See Java Native Interface at
http://docs.oracle.com/javase/8/docs/technotes/guides/jni/index.html

C语言的头文件, 用于支持JNI(Java Native Interface)编程与Java虚拟机(JVM)调试器接口编程。请参考 Java Native Interface 文档:  
[http://docs.oracle.com/javase/8/docs/technotes/guides/jni/index.html](http://docs.oracle.com/javase/8/docs/technotes/guides/jni/index.html)



See also Java Platform Debugger Architecture (JPDA) at
http://docs.oracle.com/javase/8/docs/technotes/guides/jpda/index.html

参见Java平台调试器体系结构(JPDA, Java Platform Debugger Architecture):  
[http://docs.oracle.com/javase/8/docs/technotes/guides/jpda/index.html](http://docs.oracle.com/javase/8/docs/technotes/guides/jpda/index.html)


> /jdk1.8.0/man



Contains man pages for the JDK tools.

包含JDK tools 的手册页面。


原文链接: [http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jdkfiles.html](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jdkfiles.html)





