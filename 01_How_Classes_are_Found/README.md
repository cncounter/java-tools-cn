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

引导类就是Java SE的实现类。位于 rt.jar 和 jre/lib 目录下的其他几个 JAR 文件中. 这些文件由 `sun.boot.class.path` 属性所指定,叫做 **bootstrap class path**。 这个系统属性只能引用, 不应直接修改。一般来说您不需要重新指定引导类路径. 有一个非标准选项, `-Xbootclasspath`、允许你在极端情况下指定使用其他地方的核心类。


Note that the classes that implement the JDK tools are in a separate archive from the bootstrap classes. The tools archive is the JDK /lib/tools.jar file. The development tools add this archive to the user class path when invoking the launcher. However, this augmented user class path is only used to execute the tool. The tools that process source code, the javac command, and the javadoc command, use the original class path, not the augmented version. For more information, see How the javac and javadoc Commands Find Classes.

注意, 实现了JDK工具的类和引导类位于不同的jar文件之中。工具类位于 `/lib/tools.jar` 文件. 开发工具在启动时将这个jar添加到用户类路径。然而, 这种增强的用户类路径只用于执行工具. 而处理源代码的工具, 例如 `javac` 命令,和 `javadoc` 命令,使用的是原始的类路径, 而不是增强版本. 有关更多信息,请参考下面的 [javac和javadoc命令如何找到类]()。


## How the Java Runtime Finds Extension Classes

## Java运行时如何发现扩展类


Extension classes are classes that extend the Java platform. Every .jar file in the extension directory, jre/lib/ext, is assumed to be an extension and is loaded with the Java Extension Framework. Loose class files in the extension directory are not found. They must be contained in a JAR file or Zip file. There is no option provided for changing the location of the extension directory.

扩展类(Extension classes)是扩展Java平台的类。每一个位于 `jre/lib/ext` 目录下的 jar 文件, 都被当做扩展类, 由 Java Extension Framework 负责加载. 没有打成 jar 包的 class 文件不会被加载。他们必须包含在一个JAR文件或Zip文件中。没有任何选项来改变扩展目录的位置。


If the jre/lib/ext directory contains multiple JAR files, and those files contain classes with the same name such as in the following example, the class that actually gets loaded is undefined.

如果 `jre/lib/ext` 目录下包含多个JAR文件, 而这些文件中又包含了具有相同名称的类, 例如下面的例子, 则实际加载到哪一个是不确定的。


> smart-extension1_0.jar contains class smart.extension.Smart
>
> smart-extension1_1.jar contains class smart.extension.Smart


## How the Java Runtime Finds User Classes

## Java运行时如何发现用户类


To find user classes, the launcher refers to the user class path, which is a list of directories, JAR files, and Zip files that contain class files.

加载器根据用户类路径来查找用户类, 这是一个包含类文件的列表, 其中可以包含 目录, JAR文件和Zip文件。


A class file has a subpath name that reflects the fully-qualified name of the class. For example, if the class com.mypackage.MyClass is stored under myclasses, then myclasses must be in the user class path, and the full path to the class file must be /myclasses/com/mypackage/MyClass.class on Oracle Solaris or in \myclasses\com\mypackage\MyClass.class on Windows.

包含子路径的类文件名字反映了类的完全限定名称。例如,如果类 `com.mypackage.MyClass` 存储在 myclasses 目录下, 那么必须将 `myclasses` 设置到用户类路径之中, 而且类文件的完整路径, 在Linux或者 Unix下必须是 `/myclasses/com/mypackage/MyClass.class`。在Windows上 必须是`\myclasses\com\mypackage\MyClass.class`。


If the class is stored in an archive named myclasses.jar, then myclasses.jar must be in the user class path, and the class file must be stored in the archive as com/mypackage/MyClass.class on Windows or in com\mypackage\MyClass.class on Oracle Solaris.

如果类被打包在  myclasses.jar 文件中, 那么 ` myclasses.jar` 必须加入到用户类路径中, 而 class 文件也必须存放在 jar 文件的 `com/mypackage/MyClass.class` 位置。


The user class path is specified as a string, with a colon (:) to separate the class path entries on Oracle Solaris, and a semicolon (;) to separate the entries on Windows systems. The Java launcher puts the user class path string in the java.class.path system property. The possible sources of this value are:

用户类路径通过一个字符串来指定, 在Oracle Solaris或者Linux上通过英文冒号(:)来分隔, 在Windows系统上通过英文分号(;)来分隔. Java启动程序将用户类路径字符串设置到给`java.class.path` 这个系统属性。这个值可能的来源有:


* The default value, *.*, which means that user class files are all the class files in or under the current directory.

* 默认值 `*.*` ,意思是当前目录下的所有 class 文件都是用户类。 

* The value of the CLASSPATH environment variable that overrides the default value.

* 环境变量 **CLASSPATH** 如果有值,则会覆盖默认值。


* The value of the -cp or -classpath command-line option that overrides both the default value and the CLASSPATH value.

* 命令行选项 `-cp` 或者 `-classpath` 的值,会覆盖 默认值以及 CLASSPATH 的值。


* The JAR archive specified by the -jar option overrides all other values if it contains a Class-Path entry in its manifest. If this option is used, all user classes must come from the specified archive.


* 通过 `-jar` 选项指定的 JAR 文件, 如果其 manifest 中指定了`Class-Path` 条目, 则会覆盖所有其他的值. 如果是这样, 那么所有的用户类都必须来自指定的jar文件。


## How the Java Runtime Finds JAR-class-path Classes

## Java运行时如何发现 JAR-class-path 的类


A JAR file usually contains a manifest, which is a file that lists the contents of the JAR file. The manifest can define a JAR class path, which further extends the class path, but only while loading classes from that JAR file. Classes accessed by a JAR class path are found in the following order:

通常每个JAR文件包含一个 manifest 清单, 清单文件中列出了JAR文件的内容.  manifest 可以定义JAR  class path, 从而进一步扩展 class path, 但这只适用于从该 JAR 包加载类的时候, 由JAR class path 引用的类通过以下顺序进行查找:


1. In general, classes referenced by a JAR class path entry are found as though they are part of the JAR file. The JAR files that appear in the JAR-class-path are searched after any earlier class path entries and before any entries that appear later in the class path.

1. 一般来说,JAR class path 条目引用的类路径都是JAR文件中的一部分.出现在 JAR-class-path 中的JAR文件,搜索顺序也是由其出现的顺序决定。


2. If the JAR class path points to a JAR file that was already searched, for example, an extension or a JAR file that was listed earlier in the class path, then that JAR file is not searched again. This optimization improves efficiency and prevents circular searches. This type of JAR file is searched at the point that it appears, which is earlier in the class path.

2. 如果 JAR class path 指向一个已经查找过的JAR文件,例如,扩展类或者前面的 class path 中列出的类路径, 则 JAR 文件不会被再次搜索. 这种优化提高了效率,并且阻止了循环搜索。这种类型的JAR文件在出现时进行搜索, 早于 class path。


3. If a JAR file is installed as an extension, then any JAR class path it defines is ignored. All of the classes required by an extension are presumed to be part of the JDK or to have been installed as extension.

3. 如果JAR文件被安装为扩展, 那么指定的所有JAR文件都会被忽略. 扩展类所需的任何类, 都被假定为是JDK的一部分,或者是已经安装为一个扩展。


## How the javac and javadoc Commands Find Classes

## javac和javadoc命令如何找到类

The javac and javadoc commands use class files in the following two ways:

javac和javadoc命令通过以下两种方式使用类文件:

* To run, the javac and javadoc commands must load various class files.

* 用来运行,javac和javadoc命令必须加载各种不同的类文件。


* To process the source code they operate on, the javac and javadoc commands must obtain information on object types used in the source code.

* 要处理源代码, javac和javadoc命令必须获得源代码中使用的对象的类型信息。


The class files used to resolve source code references are mostly the same class files used to run the javac and javadoc commands. But there are some important exceptions, as follows:

用于解决源代码引用的类文件大多和用于运行javac和javadoc命令的是同样的 class 文件。但也有一些重要的例外:


* Both the javac and javadoc commands often resolve references to classes and interfaces that have nothing to do with the implementation of the javac or javadoc commands. Information on referenced user classes and interfaces might be present in the form of class files, source code files, or both.

* javac和javadoc命令通常引用的类和接口都是和 javac或javadoc命令的实现无关的类. 信息通常存在于引用的用户类/接口的 class 文件, 源代码文件,或两者兼而有之。


* The tools classes in the tools.jar file are only used to run the javac and javadoc commands. The tools classes are not used to resolve source code references unless the tools.jar file is in the user class path.

* tools.jar 中的工具类只用于运行javac和javadoc命令。tools classes 一般不用于解决源码中的引用, 除非 tools.jar 文件出现在用户类路径之。


* A programmer might want to resolve boot class or extension class references with an alternative Java platform implementation. Both the javac and javadoc commands support this with their -bootclasspath and -extdirs options. Use of these options does not modify the set of class files used to run the javac or javadoc commands themselves.

* 程序员可能会使用另一种Java平台实现来解决引导类或扩展类引用. javac 和 javadoc命令支持 `-bootclasspath` 和 `-extdirs` 选项. 使用这些选项不会修改运行 javac或javadoc命令自身的 class 文件集合。


If a referenced class is defined in both a class file and a source file, the javadoc command always uses the source file. The javadoc command never compiles source files. In the same situation the javac command uses class files, but automatically recompiles any class files it determines to be out of date. The rules for automatic recompilation are documented in javac.

如果某个被引用的类中出现在 class 文件之中, 也出现在源文件之中, 则 javadoc 命令总是使用源文件。而 javadoc命令绝不编译源文件(因为他只是一个文档工具而已). 同样的情况下, javac命令使用 class 文件, 但自动重新编译那些他认为陈旧的 class 文件。自动重新编译的规则请参考 [javac 文档](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/javac.html)。


By default, the javac and javadoc commands search the user class path for both class files and source code files. If the -sourcepath option is specified, the javac and javadoc commands search for source files only on the specified source file path, while still searching the user class path for class files.

默认情况下, javac和javadoc命令查找用户类路径下的 class 文件和源代码文件. 如果指定了 `-sourcepath` 选项, javac和javadoc命令只在指定的源文件路径下搜索源文件, 同时搜索的用户类路径下的 class 文件。


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





