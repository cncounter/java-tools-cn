# 3. JDK and JRE File Structure

This chapter introduces the JDK directories and the files they contain. The file structure of the JRE is identical to the structure of the jre directory in the JDK.

This chapter covers the following topics:

- Demos and Samples

- Development Files and Directories

- Additional Files and Directories

## Demos and Samples

Demos and samples that show you how to program for the Java platform are available as a separate download at the Java SE Downloads page at
http://www.oracle.com/technetwork/java/javase/downloads/index.html

These are available as separate .tar.z compressed packages and .tar.gz compressed binaries. Similar to other 64-bit bundles on Oracle Solaris, the 64-bit demos and samples bundles on Oracle Solaris require that the 32-bit demos and samples bundles to also be installed.

## Development Files and Directories

This section describes the most important files and directories required to develop applications for the Java platform. Some of the directories that are not required include Java source code and C header files. See Additional Files and Directories.


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

### /jdk1.8.0


Root directory of the JDK software installation. Contains copyright, license, and README files. Also contains src.zip, the archive of source code for the Java platform.

### /jdk1.8.0/bin


Executables for all the development tools contained in the JDK. The PATH environment variable should contain an entry for this directory.

### /jdk1.8.0/lib


Files used by the development tools. Includes tools.jar, which contains non-core classes for support of the tools and utilities in the JDK. Also includes dt.jar, the DesignTime archive of BeanInfo files that tell interactive development environments (IDEs) how to display the Java components and how to let the developer customize them for an application.

### /jdk1.8.0/jre


Root directory of the Java Runtime Environment (JRE) used by the JDK development tools. The runtime environment is an implementation of the Java platform. This is the directory referred to by the java.home system property.

### /jdk1.8.0/jre/bin


Executable files for tools and libraries used by the Java platform. The executable files are identical to files in /jdk1.8.0/bin. The java launcher tool serves as an application launcher (and replaced the earlier jre command that shipped with 1.1 releases of the JDK). This directory does not need to be in the PATH environment variable.

### /jdk1.8.0/jre/lib

Code libraries, property settings, and resource files used by the JRE. For example rt.jar contains the bootstrap classes, which are the run time classes that comprise the Java platform core API, and charsets.jar contains the character-conversion classes. Aside from the ext subdirectory, there are several additional resource subdirectories not described here.

### /jdk1.8.0/jre/lib/ext

Default installation directory for extensions to the Java platform. This is where the JavaHelp JAR file goes when it is installed, for example. This directory includes the jfxrt.jar file, which contains the JavaFX runtime libraries and the localedata.jar file, which contains the locale data for the java.text and java.util packages. See The Extension Mechanism at
http://docs.oracle.com/javase/8/docs/technotes/guides/extensions/index.html

### /jdk1.8.0/jre/lib/security

Contains files used for security management. These include the security policy java.policy and security properties java.security files.

### /jdk1.8.0/jre/lib/sparc

Contains the .so (shared object) files used by the Oracle Solaris release of the Java platform.

### /jdk1.8.0/jre/lib/sparc/client

Contains the .so file used by the Java HotSpot VM client, which is implemented with Java HotSpot VM technology. This is the default Java Virtual Machine (JVM).

### /jdk1.8.0/jre/lib/sparc/server

Contains the .so file used by the Java HotSpot VM server.

### /jdk1.8.0/jre/lib/applet

JAR files that contain support classes for applets can be placed in the lib/applet/ directory. This reduces startup time for large applets by allowing applet classes to be preloaded from the local file system by the applet class loader and provides the same protections as though they had been downloaded over the Internet.

### /jdk1.8.0/jre/lib/fonts

Font files used by the platform.

## Additional Files and Directories

This section describes the directory structure for Java source code, C header files, and other additional directories and files.

	jdk1.8.0
	     db
	     include
	     man
	     src.zip

### /jdk1.8.0/src.zip


Archive that contains the source code for the Java platform.

### /jdk1.8.0/db

Contains Java DB. See Java DB Technical Documentation at
http://docs.oracle.com/javadb/

### /jdk1.8.0/include

C-language header files that support native-code programming with the Java Native Interface and the Java Virtual Machine (JVM) Debugger Interface. See Java Native Interface at
http://docs.oracle.com/javase/8/docs/technotes/guides/jni/index.html

See also Java Platform Debugger Architecture (JPDA) at
http://docs.oracle.com/javase/8/docs/technotes/guides/jpda/index.html

### /jdk1.8.0/man

Contains man pages for the JDK tools.


原文链接: [http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jdkfiles.html](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/jdkfiles.html)

