<font size=6 face="Times New Roman">Linux Notes</font>

***

[TOC]

***

##  1. 简介

* Linux 版本
  内核版本指的是在 Linus 领导下的开发小组开发出的系统内核的版本号。Linux 的每个内核版本使用形式为 x.y.zz-www 的一组数字来表示。其中：
  * x.y：为linux的主版本号。通常y若为奇数，表示此版本为测试版，系统会有较多bug，主要用途是提供给用户测试。
  * zz：为次版本号。
  * www：代表发行号（注意，它与发行版本号无关）。

* Linux 体系结构图
  ![Linux 体系结构图](./img/Linux体系结构图.jpg)
  
   * 内核：内核是操作系统的核心。内核直接与硬件交互，并处理大部分较低层的任务，如内存管理、进程调度、文件管理等。
   * Shell：Shell是一个处理用户请求的工具，它负责解释用户输入的命令，调用用户希望使用的程序。
   * 命令和工具：日常工作中，你会用到很多系统命令和工具，如cp、mv、cat和grep等。在Linux系统中，有250多个命令，每个命令都有多个选项；第三方工具也有很多，他们也扮演着重要角色。
   * 文件和目录：Linux系统中所有的数据都被存储到文件中，这些文件被分配到各个目录，构成文件系统。Linux的目录与Windows的文件夹是类似的概念。

* 登陆Linux
  
* 修改密码
  ```
  $ passwd
  ```
* 查看目录和文件
  ```
  $ ls -l
  ```
* 查看当前用户
  ```
  $ whoami
  ```
* 查看当前在线用户
  ```
  $ who
  ``
* 退出登陆

* 关闭系统
  |命令|说明|
  |:-:|:-:|
  |halt|直接关闭系统|
  |init 0 | 使用预定义脚本关闭系统，关闭前可以清理和更新有关信息|
  |init 6|重启|
  |poweroff|断电关闭系统|
  |reboot|重启|
  |shutdown|关机|

  一般情况下使用超级用户和root用户才有关闭系统权限。

## 2. 文件管理

* 在Linux中，有三种基本的文件类型
  1. 普通文件
    普通文件是以字节为单位的数据流，包括文本文件、源码文件、可执行文件等。文本和二进制对Linux来说并无区别，对普通文件的解释由处理该文件的应用程序进行。
  2. 目录
    目录可以包含普通文件和特殊文件，目录相当于Windows和Mac OS中的文件夹。
  3. 设备文件
    有些教程中称特殊文件，是一个含义。Linux 与外部设备（例如光驱，打印机，终端，modern等）是通过一种被称为设备文件的文件来进行通信。Linux 输入输出到外部设备的方式和输入输出到一个文件的方式是相同的。Linux 和一个外部设备通讯之前，这个设备必须首先要有一个设备文件存在。

* 查看文件
  ```
  $ ls -l

  drwxrwxr-x  2 amrood amrood      4096 Dec 25 09:59 uml
  -rw-rw-r--  1 amrood amrood      5341 Dec 25 08:38 uml.jpg
  drwxr-xr-x  2 amrood amrood      4096 Feb 15  2006 univ
  drwxr-xr-x  2 root   root        4096 Dec  9  2007 urlspedia
  -rw-r--r--  1 root   root      276480 Dec  9  2007 urlspedia.tar
  drwxr-xr-x  8 root   root        4096 Nov 25  2007 usr
  drwxr-xr-x  2    200    300      4096 Nov 25  2007 webthumb-1.01
  -rwxr-xr-x  1 root   root        3192 Nov 25  2007 webthumb.php
  -rw-rw-r--  1 amrood amrood     20480 Nov 25  2007 webthumb.tar
  -rw-rw-r--  1 amrood amrood      5654 Aug  9  2007 yourfile.mid
  -rw-rw-r--  1 amrood amrood    166255 Aug  9  2007 yourfile.swf
  drwxr-xr-x 11 amrood amrood      4096 May 29  2007 zlib-1.2.3
  $
  ```
  每一列的含义如下：
  * 第一列：文件类型。
  * 第二列：表示文件个数。如果是文件，那么就是1；如果是目录，那么就是该目录中文件的数目。
  * 第三列：文件的所有者，即文件的创建者。
  * 第四列：文件所有者所在的用户组。在Linux中，每个用户都隶属于一个用户组。
  * 第五列：文件大小（以字节计）。
  * 第六列：文件被创建或上次被修改的时间。
  * 第七列：文件名或目录名。

* 元字符
  元字符是具有特殊含义的字符。* 和 ? 都是元字符：
    * 可以匹配 0 个或多个任意字符；
    * ? 匹配一个字符。
   eg：`$ ls *.doc`：显示所有的doc结尾文件

* 隐藏文件
  隐藏文件的第一个字符为英文句号或点号(.)，Linux程序（包括Shell）通常使用隐藏文件来保存配置信息。
  下面是一些常见的隐藏文件：
  + .profile：Bourne shell (sh) 初始化脚本
  + .kshrc：Korn shell (ksh) 初始化脚本
  + .cshrc：C shell (csh) 初始化脚本
  + .rhosts：Remote shell (rsh) 配置文件

  查看隐藏文件需要 使用**ls**命令的<b>-a</b>选项
  ```
  $ ls -a
  ```
* 创建文件
  ```
  $ vi <filename>
  ```

* 编辑文件
  普通模式下，使用：
  + l 键右移
  + h 键左移
  + k 键上移
  + j 键下移

* 查看文件内容
  ```
  $ cat [-b] <filename>
  ``` 
  <b>-b</b>显示行号

* 统计单词数目
  ```
  $ wc <filename>
  2 16 199 filename
  ```
  每一列的含义如下：
  + 第一列：文件的总行数
  + 第二列：单词数目
  + 第三列：文件的字节数，即文件的大小
  + 第四列：文件名

* 复制文件
  ```
  $ cp src_file dst_file
  ```

* 重命名
  ```
  $ mv old_file new_file
  ```

* 删除文件
  ```
  $ rm <filename>
  ```

* 标准的Linux流
  一般情况下，每个Linux程序运行时都会创建三个文件流（三个文件）：
  + 标准输入流(stdin)：stdin的文件描述符为0，Linux程序默认从stdin读取数据。
  + 标准输出流(stdout)：stdout 的文件描述符为1，Linux程序默认向stdout输出数据。
  + 标准错误流(stderr)：stderr的文件描述符为2，Linux程序会向stderr流中写入错误信息。

