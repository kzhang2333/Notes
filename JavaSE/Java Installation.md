# Java - Install and config environment
## 1. uninstall java
1. 在环境变量中找到JAVA_HOME 指向的jdk位置，并删除其
2. 删除JAVA_HOME
3. Delete path 下关于java的目录
4. java -version in cmd to test if still exist

## 2. install java
1. 下载jdk8

2. 同意协议，注册

3. 下载对应版本

4. 双击安装

  **记住安装路径**

5. 配置环境变量
  - 我的电脑-->属性
  - 环境变量--> JAVA_HOME
  - path变量
  - 记得退出确定才会保存
7. 测试
  - 打开cmd
  - java -version


设置环境变量
JAVA_HOME --> jdk 安装位置
设置path变量
%JAVA_HOME%\lib;
%JAVA_HOME%\jre\bin;
以上两个path加入到下面一长串中，用分好隔开
>  C:\MinGW\bin;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Program Files (x86)\NVIDIA Corporation\PhysX\Common;C:\Program Files\Common Files\Autodesk Shared\;C:\Program Files\Intel\WiFi\bin\;C:\Program Files\Common Files\Intel\WirelessCommon\;C:\Program Files\PuTTY\;C:\Program Files\MATLAB\R2019a\bin;C:\Program Files\dotnet\;C:\Program Files\Microsoft SQL Server\130\Tools\Binn\;C:\Program Files\Microsoft SQL Server\Client SDK\ODBC\170\Tools\Binn\

## 3. JDK文件目录
1. bin - java，javac，可执行程序，java入口
2. include - java需要用到的c语言头文件，一些.h文件，这是因为java会用到c 才能调用底层
3. jre - java运行环境，用户只需jre，开发使用jdk
4. lib - 一些package
5. src.zip - java原生类库，**很多官方定义的源码在此**
