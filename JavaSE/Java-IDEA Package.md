# 用IDEA进行开发

## 什么是IDE

继承开发环境 - Integrated Development Environment

eclipse 的时代已经过去了, 互联网时代的宠儿是IDEA

jetbrains 公司 - IntelliJ IDEA 的作者

## 使用

1. new project
2. psvm - main method
3. sop - System.out.println()



## IDEA

在互联网时代IDEA比eclipse

# 包机制

包的本质就是一个文件夹， 目的是为了防止文件名冲突。

- 一般利用公司域名倒置作为包名；www.google.com --> com.google.www;
- 公司域名后还可以增加不同业务作为子包，如
  - com.google.googledoc
  - com.google.googlemap
  - ...
- package必须放在.java文件的第一行
- import可以导入包
- *通配符， 可以导入包内所有class