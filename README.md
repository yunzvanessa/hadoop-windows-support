# hadoop-windows-support
hadoop.dll and winutil.exe binaries for hadoop windows


## Requirements
- Keep the source code tree in a short path to avoid running into problems related
to Windows maximum path length limitation (for example, C:\hdc).
- There is one support command file located in dev-support called win-paths-eg.cmd.
It should be copied somewhere convenient and modified to fit your needs.
- win-paths-eg.cmd sets up the environment for use. You will need to modify this
file. It will put all of the required components in the command path,
configure the bit-ness of the build, and set several optional components. Run this file as Administartor after modifying the paths.

Example `win-paths-eg.cmd`:

```
SET Platform=x64
SET VCVARSPLAT=amd64

SET MAVEN_OPTS=-Dmaven.repo.local=C:\Tools\m2SET MAVEN_HOME=%MAVEN_HOME%
SET JAVA_HOME=%JAVA_HOME%
SET MSVS=C:\Program Files\Microsoft Visual Studio 10.0
SET PROTO_BIN=C:\Program Files\protoc-2.5.0-win32
SET GIT_HOME=C:\Program Files\Git
SET ZLIB_HOME=%ZLIB_HOME%

SET PATH=%JAVA_HOME%\bin;%MAVEN_HOME%\bin;%PROTO_BIN%;%GIT_HOME%\bin;%ZLIB_HOME%\bin;%PATH%

CALL "%MSVS%\VC\vcvarsall.bat" %VCVARSPLAT%
```

## Build
Apache Hadoop repo:

```
https://github.com/apache/hadoop
```

Under the cloned directory `HADOOP_HOME`, run the following command:

```
mvn package -Pdist,native-win -DskipTests -Dtar -Dmaven.javadoc.skip=true
```

## Versions

### Hadoop 3.3.0

#### Environment setup
- Windows 2019
- Java JDK 1.8.0_72
- Maven 3.6.3
- protoc-2.5.0-win32
- CMake 3.24.1
- Visual Studio 2010 Professional
- Windows SDK 7.1
- zlib-1.2.3
- git 2.21.0
- node 16.17.0
- npm 8.15.0

#### Possible errors and solutions
**Error 1**

```
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (make) 
on project hadoop-hdfs-native-client: An Ant BuildException has occured: exec returned: 1

[ERROR] around Ant part ...<exec failonerror="true" dir="D:\h\hadoop\hadoop-hdfs-project
\hadoop-hdfs-native-client\target/native" executable="cmake">... @ 5:125 in 
C:\hadoop\hadoop-hdfs-project\hadoop-hdfs-native-client\target\antrun\build-main.xml
```
**Solution**

In file `C:\hadoop\hadoop-hdfs-project\hadoop-hdfs-native-client\pom.xml`, change `failonerror="true"` to `failonerror="false"`.


**Error 2**	

```
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (make) on 
project hadoop-hdfs-native-client: An Ant BuildException has occured: 
C:\hadoop\hadoop-hdfs-project\hadoop-hdfs-native-client\target\native\bin\RelWithDebInfo does not exist.

[ERROR] around Ant part ...<copy todir="C:\hadoop\hadoop-hdfs-project\hadoop-hdfs-native-client\target/bin">... 
@ 13:86 in D:\h\hadoop\hadoop-hdfs-project\hadoop-hdfs-native-client\target\antrun\build-main.xml

```		

**Solution**

Create empty directory `C:\hadoop\hadoop-hdfs-project\hadoop-hdfs-native-client\target\native\bin\RelWithDebInfo`


**Error 3**

```
[INFO] [2/4] Fetching packages...
[INFO] error winston@3.7.2: The engine "node" is incompatible with this module. 
Expected version ">= 12.0.0".
[INFO] error Found incompatible module
[INFO] info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
[INFO] ------------------------------------------------------------------------
```

**Solution**

In file `C:\hadoop\hadoop-yarn-project\hadoop-yarn\hadoop-yarn-applications\hadoop-yarn-applications-catalog\hadoop-yarn-applications-catalog-webapp\pom.xml`, change `nodeVersion` to `v16.17.0`

## Reference
- [https://github.com/kontext-tech/winutils]()
- [https://www.jianshu.com/p/1b4cbabfd899]() (Simplified Chinese)
- [https://blog.csdn.net/clj198606061111/article/details/106415942]()  (Simplified Chinese)
- [https://www.cnblogs.com/bclshuai/p/12009991.html]() (Simplified Chinese)