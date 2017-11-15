
# javap

Disassembles one or more class files.

展开(Disassembles)一到多个 `class` 文件

## javap概述(Synopsis)

> **javap** _options_ _classfile_ ...

- _options_

The command-line options. See Options.

命令行参数, 请参考下方的选项(Options)部分。 

- _classfile_

One or more classes separated by spaces to be processed for annotations such as `DocFooter.class`. You can specify a class that can be found in the class path, by its file name or with a URL such as `file:///home/user/myproject/src/DocFooter.class`.

参数为一到多个 class 文件, 使用空格分隔, 例如 `DocFooter.class`。可以是 class path 中的某个类, 例如文件名或者是文件URL, 如 `file:///home/user/myproject/src/DocFooter.class`。




## 说明(Description)

The `javap` command disassembles one or more class files. The output depends on the options used. When no options are used, then the `javap` command prints the package, protected and public fields, and methods of the classes passed to it. The `javap` command prints its output to `stdout`.

`javap` 命令展开一个/或多个 class 文件。可以通过命令选项控制输出的具体内容。如果不指定任何选项, 则打印出 package, protected 和 public 字段(fields), 以及参数中指定 class 的方法。 `javap`命令的结果内容输出到 `stdout`。



## 选项(Options)

* `-help`, `--help`, `-?`
  打印出 `javap` 命令的帮助信息.

* `-version`
  打印出 `javap` 命令的版本(release)信息.
* `-l`
  打印出行号(line)以及局部变量表(local variable tables).
* `-public`
  只显示 public 类和成员.
* `-protected`
  只显示 protected/public 类和成员.
* `-private`, `-p`
  显示所有的类和成员(all classes and members).
* `-J_xxxoption_`
  传递给底层JVM的参数. 例如:
  ```
  javap -J-version
  javap -J-Djava.security.manager -J-Djava.security.policy=MyPolicy MyClassName
  ```
  更多 JVM option 的信息, 请参考 `[java`](05_04_java.md) 命令的官方文档.
* `-s`
  打印内部类型签名(internal type signatures).
* `-sysinfo`
  显示class文件的系统信息(如文件路径(path), 大小(size), 修改日期(date), 以及MD5 hash).
* `-constants`
  显示 `static final` 常量信息.
* `-c`
  打印反编译后的代码(disassembled code), 例如, 以指令(instructions)的方式展示 class 中每个方法的字节码.
* `-verbose`
  打印 stack size, 局部变量number of locals and arguments for methods.
* `-classpath _path_`
  指定 `javap` 命令查找 class 时所使用的 classpath 路径. 如果指定该参数, 则会覆盖默认的系统环境变量 `CLASSPATH` 值.
* `-bootclasspath _path_`
  指定加载 bootstrap 类的路径. 默认情况下, bootstrap classes 从Java安装目录下的  `jre/lib/rt.jar` 以及其他几个 JAR 包中加载.
* `-extdir _dirs_`
  手工指定安装 extensions 的查找目录. 默认 extensions 目录由 `java.ext.dirs` 的值指定.


## Example

Compile the following `DocFooter` class:

```
import java.awt.*;
import java.applet.*;

public class DocFooter extends Applet {
        String date;
        String email;

        public void init() {
                resize(500,100);
                date = getParameter("LAST_UPDATED");
                email = getParameter("EMAIL");
        }

        public void paint(Graphics g) {
                g.drawString(date + " by ",100, 15);
                g.drawString(email,290,15);
        }
}

```

The output from the `javap DocFooter.class` command yields the following:

```
Compiled from "DocFooter.java"
public class DocFooter extends java.applet.Applet {
  java.lang.String date;
  java.lang.String email;
  public DocFooter();
  public void init();
  public void paint(java.awt.Graphics);
}

```

The output from `javap -c DocFooter.class` command yields the following:

```
Compiled from "DocFooter.java"
public class DocFooter extends java.applet.Applet {
  java.lang.String date;
  java.lang.String email;

  public DocFooter();
    Code:
       0: aload_0       
       1: invokespecial #1                  // Method
java/applet/Applet."<init>":()V
       4: return        

  public void init();
    Code:
       0: aload_0       
       1: sipush        500
       4: bipush        100
       6: invokevirtual #2                  // Method resize:(II)V
       9: aload_0       
      10: aload_0       
      11: ldc           #3                  // String LAST_UPDATED
      13: invokevirtual #4                  // Method
 getParameter:(Ljava/lang/String;)Ljava/lang/String;
      16: putfield      #5                  // Field date:Ljava/lang/String;
      19: aload_0       
      20: aload_0       
      21: ldc           #6                  // String EMAIL
      23: invokevirtual #4                  // Method
 getParameter:(Ljava/lang/String;)Ljava/lang/String;
      26: putfield      #7                  // Field email:Ljava/lang/String;
      29: return        

  public void paint(java.awt.Graphics);
    Code:
       0: aload_1       
       1: new           #8                  // class java/lang/StringBuilder
       4: dup           
       5: invokespecial #9                  // Method
 java/lang/StringBuilder."<init>":()V
       8: aload_0       
       9: getfield      #5                  // Field date:Ljava/lang/String;
      12: invokevirtual #10                 // Method
 java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      15: ldc           #11                 // String  by 
      17: invokevirtual #10                 // Method
 java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      20: invokevirtual #12                 // Method
 java/lang/StringBuilder.toString:()Ljava/lang/String;
      23: bipush        100
      25: bipush        15
      27: invokevirtual #13                 // Method
 java/awt/Graphics.drawString:(Ljava/lang/String;II)V
      30: aload_1       
      31: aload_0       
      32: getfield      #7                  // Field email:Ljava/lang/String;
      35: sipush        290
      38: bipush        15
      40: invokevirtual #13                 // Method
java/awt/Graphics.drawString:(Ljava/lang/String;II)V
      43: return        
}

```

## See Also

*   [`java`(1)](05_04_java.md)

*   [`javac`(1)](javac.html#BHCJCBFB)

*   [`javadoc`(1)](javadoc.html#CHDFCDCI)

*   [`javah`(1)](javah.html#BJECIACA)

*   [`jdb`(1)](jdb.html#CHDFHFDB)

*   [`jdeps`(1)](jdeps.html#BACEHAGD)

