# 1. Java如何找到Class


The java command is called the Java launcher because it launches Java applications. When the Java launcher is called, it gathers input from the user and the user's environment (such as the class path and the boot class path), interfaces with the Virtual Machine (VM), and gets it started via some bootstrapping. The Java Virtual Machine (JVM) does the rest of the work.

`java` 命令被称为 Java launcher, 因为它启动java应用. 当调用 Java launcher 启动程序时, 它收集用户输入以及用户环境(如类路径(class path)和引导类路径(boot class path) ), 虚拟机(VM)接口, 并通过一些引导来开始。剩下的工作由Java虚拟机(JVM)负责。


This chapter covers the following topics:

本章包括以下主题:


How the Java Runtime Finds Classes

Java运行时如何发现类


How the javac and javadoc Commands Find Classes

javac 和 javadoc 命令如何发现类


Class Loading and Security Policies

类加载和安全策略


## How the Java Runtime Finds Classes

## Java运行时如何发现类


The JVM searches for and loads classes in this order:

JVM以下面的顺序来查找和加载类:


1. Bootstrap classes, which are classes that comprise the Java platform, including the classes in rt.jar and several other important JAR files.

1. 引导类(Bootstrap classes), 由Java平台组成, 包括 `rt.jar` 和其他几个重要的JAR文件中的类。


2. Extension classes, which use the Java Extension mechanism. These classes are bundled as JAR files and located in the extensions directory.

2. 扩展类(Extension classes), 使用Java扩展机制(Extension mechanism)。这些类被打包为JAR文件, 放在扩展目录中。


3. User classes are classes that are defined by developers and third parties and that do not take advantage of the extension mechanism. You identify the location of these classes with the -classpath option on the command line (preferred) or with the CLASSPATH environment variable. See [Setting the Class Path](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/classpath.html#CBHHCGFB).

3. 用户类(User classes), 是由开发人员和第三方定义的类, 不利用扩展机制. 你可以通过命令行设置 `-classpath` 选项来确定这些类的位置(优先). 还可以设置 **CLASSPATH** 环境变量。可以参考 [Setting the Class Path](../02_Setting_the_Class_Path/)。


In effect, the three search paths together form a simple class path. This is similar to the flat class path previously used, but the current model has the following improvements:

实际上,这三个搜索路径组成一个简单的类路径。这类似于之前使用的平坦类路径,但目前的模型有以下改进:


* It is relatively difficult to accidentally hide or omit the bootstrap classes.

* 相对很难隐藏或省略引导类。


* In general, you only have to specify the location of user classes. Bootstrap classes and extension classes are found automatically.

* 在一般情况下,您只需要指定用户类的位置。引导类和扩展类会自动发现。


* The tools classes are now in a separate archive (tools.jar) and can only be used if included in the user class path described in How the Java Runtime Finds Bootstrap Classes.

* 工具类现在在一个单独的包中(`tools.jar`), 只有包含在用户类路径中才能使用, 参考下面的 **Java运行时如何发现引导类**。


## How the Java Runtime Finds Bootstrap Classes


## Java运行时如何发现引导类


Bootstrap classes are the classes that implement Java SE. Bootstrap classes are in the rt.jar file and several other JAR files in the jre/lib directory. These archives are specified by the value of the bootstrap class path that is stored in the sun.boot.class.path system property. This system property is for reference only and should not be directly modified.It is unlikely that you will need to redefine the bootstrap class path. The nonstandard option, -Xbootclasspath, allows you to do so in those rare circumstances in which it is necessary to use a different set of core classes.

引导类的类实现Java SE。引导rt.jar文件中的类和其他几个JAR文件在jre / lib目录中.这些档案规定的值存储在sun.boot.class引导类路径。道路系统属性.这个系统属性仅供参考,不应直接修改。您将需要重新定义不太可能引导类路径.《nonstandard备选办法,-Xbootclasspath、致you to do na in身上罕见circumstances囚犯在没有必要使用了不同的完整core职等。


Note that the classes that implement the JDK tools are in a separate archive from the bootstrap classes. The tools archive is the JDK /lib/tools.jar file. The development tools add this archive to the user class path when invoking the launcher. However, this augmented user class path is only used to execute the tool. The tools that process source code, the javac command, and the javadoc command, use the original class path, not the augmented version. For more information, see How the javac and javadoc Commands Find Classes.

注意,在一个单独的类,实现JDK工具从引导类档案。档案是JDK /lib/tools.的工具jar文件.开发工具将这个档案添加到用户类路径调用发射器。然而,这种增强用户类路径是仅用于执行工具.过程的工具源代码,javac命令,和javadoc命令,使用原始的类路径,而不是增强版本.有关更多信息,请参见如何javac和javadoc命令找到类。


## How the Java Runtime Finds Extension Classes

## Java运行时发现扩展类如何


Extension classes are classes that extend the Java platform. Every .jar file in the extension directory, jre/lib/ext, is assumed to be an extension and is loaded with the Java Extension Framework. Loose class files in the extension directory are not found. They must be contained in a JAR file or Zip file. There is no option provided for changing the location of the extension directory.

扩展类的类扩展Java平台。每一个。扩展目录中的jar文件,jre / lib / ext,被认为是一个扩展和加载的Java扩展框架.宽松的扩展目录中的类文件没有找到。他们必须包含在一个JAR文件或Zip文件。没有提供选项改变扩展目录的位置。


If the jre/lib/ext directory contains multiple JAR files, and those files contain classes with the same name such as in the following example, the class that actually gets loaded is undefined.

如果jre / lib / ext目录下包含多个JAR文件,这些文件包含具有相同名称的类,如在下面的例子中,实际加载的类定义。


> smart-extension1_0.jar contains class smart.extension.Smart
>
>smart-extension1_1.jar contains class smart.extension.Smart


## How the Java Runtime Finds User Classes

## Java运行时发现用户类如何


To find user classes, the launcher refers to the user class path, which is a list of directories, JAR files, and Zip files that contain class files.

找到用户类,发射器是指用户类路径,这是一个目录列表,JAR文件和Zip文件包含类文件。


A class file has a subpath name that reflects the fully-qualified name of the class. For example, if the class com.mypackage.MyClass is stored under myclasses, then myclasses must be in the user class path, and the full path to the class file must be /myclasses/com/mypackage/MyClass.class on Oracle Solaris or in \myclasses\com\mypackage\MyClass.class on Windows.

一个类文件的子路径的名字反映了类的完全限定名称。例如,如果类com.mypackage.MyClass存储在MyClass,那么必须在用户MyClass类路径,和类文件的完整路径必须/ MyClass /com/mypackage/MyClass.类在Oracle Solaris或\ MyClass \ com \ mypackage \ MyClass。类在Windows。


If the class is stored in an archive named myclasses.jar, then myclasses.jar must be in the user class path, and the class file must be stored in the archive as com/mypackage/MyClass.class on Windows or in com\mypackage\MyClass.class on Oracle Solaris.

如果类存储在一个名为myclass的档案。jar,然后myclass。jar必须在用户类路径中,必须存储在类文件归档com/mypackage/MyClass.类在Windows或com \ mypackage \ MyClass。类在Oracle Solaris。


The user class path is specified as a string, with a colon (:) to separate the class path entries on Oracle Solaris, and a semicolon (;) to separate the entries on Windows systems. The Java launcher puts the user class path string in the java.class.path system property. The possible sources of this value are:

用户类路径指定为一个字符串,用冒号(:)在Oracle Solaris,单独的类路径条目和分号(;)来分离在Windows系统上的条目.Java启动程序将java.class用户类路径字符串。道路系统属性。这个值的可能的来源有:


* The default value, *.*, which means that user class files are all the class files in or under the current directory.

* The value of the CLASSPATH environment variable that overrides the default value.


* The value of the -cp or -classpath command-line option that overrides both the default value and the CLASSPATH value.

* The JAR archive specified by the -jar option overrides all other values if it contains a Class-Path entry in its manifest. If this option is used, all user classes must come from the specified archive.


* 指定的JAR归档JAR选项覆盖所有其他值如果它包含一个类路径条目清单.如果使用这个选项,所有用户类必须来自指定的档案。


## How the Java Runtime Finds JAR-class-path Classes

## Java运行时发现JAR-class-path类如何


A JAR file usually contains a manifest, which is a file that lists the contents of the JAR file. The manifest can define a JAR class path, which further extends the class path, but only while loading classes from that JAR file. Classes accessed by a JAR class path are found in the following order:

一个JAR文件通常包含一个清单,这是一个文件,列出了JAR文件的内容.可以定义为JAR manifest The class路径,从而进一步extends The class路径,但这仅而classes loading JAR file,从.类访问一罐类路径中发现以下订单:


1. In general, classes referenced by a JAR class path entry are found as though they are part of the JAR file. The JAR files that appear in the JAR-class-path are searched after any earlier class path entries and before any entries that appear later in the class path.

1. 一般来说,类JAR引用的类路径条目发现像JAR文件的一部分.JAR文件后,出现在搜索JAR-class-path任何类路径条目,条目之前早些时候出现在类路径中。


2. If the JAR class path points to a JAR file that was already searched, for example, an extension or a JAR file that was listed earlier in the class path, then that JAR file is not searched again. This optimization improves efficiency and prevents circular searches. This type of JAR file is searched at the point that it appears, which is earlier in the class path.

2. 如果JAR的类路径点已经搜查了一个JAR文件,例如,一个扩展或JAR文件前面列出的类路径中,然后再次JAR文件不是搜索.这种优化提高效率和防止循环搜索。这种类型的JAR文件搜索的时候,似乎,这是早些时候在类路径中。


3. If a JAR file is installed as an extension, then any JAR class path it defines is ignored. All of the classes required by an extension are presumed to be part of the JDK or to have been installed as extension.

3. 如果一个JAR文件安装为一个扩展,然后定义忽略任何JAR的类路径.所需的所有类的扩展被假定是JDK的一部分或者是安装扩展。


## How the javac and javadoc Commands Find Classes
The javac and javadoc commands use class files in the following two ways:

## javac和javadoc命令如何找到类
使用javac和javadoc命令类文件在以下两个方面:


* To run, the javac and javadoc commands must load various class files.

* 运行,javac和javadoc命令必须加载不同的类文件。


* To process the source code they operate on, the javac and javadoc commands must obtain information on object types used in the source code.

* 处理他们操作的源代码,javac和javadoc命令必须获得信息对象类型中使用源代码。


The class files used to resolve source code references are mostly the same class files used to run the javac and javadoc commands. But there are some important exceptions, as follows:

用于解决源代码引用的类文件大多是同一类文件用于运行javac和javadoc命令。但也有一些重要的例外,如下:


* Both the javac and javadoc commands often resolve references to classes and interfaces that have nothing to do with the implementation of the javac or javadoc commands. Information on referenced user classes and interfaces might be present in the form of class files, source code files, or both.

* javac和javadoc命令通常解决引用类和接口无关的javac或javadoc命令的实现.信息可能存在引用用户类和接口的类文件,源代码文件,或两者兼而有之。


* The tools classes in the tools.jar file are only used to run the javac and javadoc commands. The tools classes are not used to resolve source code references unless the tools.jar file is in the user class path.

* 工具类的工具。jar文件仅用于运行javac和javadoc命令。工具类不习惯解决源代码引用,除非工具.jar文件在用户类路径。


* A programmer might want to resolve boot class or extension class references with an alternative Java platform implementation. Both the javac and javadoc commands support this with their -bootclasspath and -extdirs options. Use of these options does not modify the set of class files used to run the javac or javadoc commands themselves.

* 程序员可能想解决引导类或扩展类引用另一个Java平台实现.javac和javadoc命令支持这个bootclasspath和-extdirs选项.使用这些选项不修改类文件的设置用于运行javac或javadoc命令自己。


If a referenced class is defined in both a class file and a source file, the javadoc command always uses the source file. The javadoc command never compiles source files. In the same situation the javac command uses class files, but automatically recompiles any class files it determines to be out of date. The rules for automatic recompilation are documented in javac.

如果一个引用类中定义一个类文件和源文件,javadoc命令总是使用源文件。javadoc命令编译源文件.在相同的情况下,javac命令使用类文件,但自动重新编译类文件它决定要过时了。的规则自动重新编译在javac记录。


By default, the javac and javadoc commands search the user class path for both class files and source code files. If the -sourcepath option is specified, the javac and javadoc commands search for source files only on the specified source file path, while still searching the user class path for class files.

默认情况下,javac和javadoc命令搜索的用户类路径类文件和源代码文件.如果指定的路径中选择,javac和javadoc命令搜索源文件只在指定的源文件路径,同时搜索的用户类路径类文件。


## Class Loading and Security Policies

## 类加载和安全策略

To be used, a class or interface must be loaded by a class loader. Use of a particular class loader determines a security policy associated with the class loader.


要使用类加载和安全策略, 必须通过 class loader 来加载一个类或接口。使用一个特定的类加载器就确定了与类加载器相关联的一个安全策略。


A program can load a class or interface by calling the loadClass method of a class loader object. But usually a program loads a class or interface by referring to it. This invokes an internal class loader, which can apply a security policy to extension and user classes. If the security policy has not been enabled, all classes are trusted. Even if the security policy is enabled, it does not apply to bootstrap classes, which are always trusted.

程序可以通过类加载器对象的 `loadClass` 方法来加载一个类或接口。但通常通过指向一个类来加载一个类或接口. 这会调用内部的类加载器, 其可以将安全策略应用于扩展类和用户类。如果安全策略尚未启用, 所有类都是可信的. 即使启用了安全策略,它也不适用于引导类,因为总是信任 bootstrap classes。


When enabled, security policy is configured by system and user policy files. The Java platform SDK includes a system policy file that grants trusted status to extension classes and places basic restrictions on user classes.

启用时,安全策略通过系统和用户策略文件配置. Java平台的SDK包含一个系统策略文件, 授予对扩展类的信任状态，以及对用户类给予基本限制。


To enable or configure the security policy, refer to Security Features.

启用或配置安全策略, 请参阅 [安全特性]()。


Note: Some security programming techniques that worked with earlier releases of Java SE are incompatible with the class loading model of the current release.

> **注意**: 早期Java SE版本的一些安全编程技术和当前版本的类加载模型是不兼容的。


原文链接: [http://docs.oracle.com/javase/8/docs/technotes/tools/windows/findingclasses.html](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/findingclasses.html)





