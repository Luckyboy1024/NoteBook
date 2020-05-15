## 前言


本篇博客意在讲解关于 linux 的七种文件类型、文件的属性及查找方法、文件的索引节点(inode)  

------



@[toc]( )

------


## Linux 下分为七种文件类型

> Linux 下，一切皆文件!!!

### 普通文件
**文件第一个属性**：[ `-` ]，例：[`-rw-rw-r--`]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515154755627.png)
Linux中最多的一种文件类型, 包括
- 纯文本文件(ASCII)；
- 二进制文件(binary)；
- 数据格式的文件(data);
- 各种压缩文件。

-----

### 目录文件
**文件第一个属性**：[ `d` ]，例：[`drwxrwxr-x`]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515154851198.png)
在linux中，一切皆是文件，目录文件也就是Windows中的目录，能用 cd 命令进入目录文件。

-----

### 字符设备文件
**文件第一个属性**：[ `c` ]，例：[`crw-rw----`]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515155343970.png)
即串行端口的接口设备，例如键盘、鼠标等等。

------

### 块设备文件
**文件第一个属性**：[ `b` ]，例：[`brw-rw----`]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515160642754.png)
块设备，即存储数据以供系统存取的接口设备，简单而言就是硬盘。例如 /dev/sda 等文件。

----

### 套接字文件
**文件第一个属性**：[ `s` ]，例：[`srw-rw-rw-`]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515161419922.png)
这类文件通常用在网络数据连接。可以启动一个程序来监听客户端的要求，客户端就可以通过套接字来进行数据通信。最常在 /var/run/ 目录中看到这种文件类型。

### 管道文件
**文件第一个属性**：[ `p` ]，例：[`prw-rw----`]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515162416254.png)
管道 (FIFO文件) 也是一种特殊的文件类型，它主要的目的是：解决多个程序同时存取一个文件所造成的错误。FIFO 是 first-in-first-out (先进先出) 的缩写。

### 链接文件
**文件第一个属性**：[ `l` ]，例：[`lrwxrwxrwx`]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515162510730.png)
类似于 Windows 中的快捷方式。


## 查看文件类型三种方式
- 使用 `ll` 或者 `ls -l`，看显示内容的第一个字符
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515163726984.png)
- 使用 `file` 命令，如 file file.txt
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515163023338.png)
- 使用 `stat` 命令，查看文件的详细信息。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515163913740.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)

注：另外，如果想查看文件或目录的大小：`du [filename]`，例如 du file.txt
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515164109456.png)

----

## 文件后缀的小作用

Linux 中文件的文件类型和文件名，不同于 Windows 中文件是依靠文件后缀名来区分的文件类型，在 linux 中文件类型与文件扩展名没有关系，但为了用户的使用习惯和使用方便，我们还是会规定一些扩展名来表示文件类型。举例如下：
- `.tar`、`.tar.gz`、`.tgz`、`.zip`、`.tar.bz` 表示压缩文件，压缩指令：tar, gzip, zip 等;
- `.sh` 表示 shell 脚本文件;
- `.py` 表示 python 文件，通过 python 语言开发的程序;
- `.pl` 表示 perl 文件，通过 perl 语言开发的程序;
- `.html`、`.htm`、`.php`、`.jsp`、`.do` 表示网页语言的文件;
- `.conf` 表示系统服务的配置文件。
- `.rpm` 表示 rpm 安装包文件。

----

## 文件的属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515172310725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)
举个实例来演示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515172714707.png)
**inode 索引节点编号**：`102041285`
**文件类型**：文件类型是 [`-`]，表示这是一个普通文件
**文件权限**：[`rw-rw-r--`] 表示文件所属用户可读、可写、不可执行，文件所归属的用户组可读可写，其他用户只可读
**硬链接个数**：[`1`] 表示 file 这个文件没有其他的硬链接，因为连接数 1 就是他本身
**文件所属用户**：表示这个文件所属的用户，这里的意思是 file 文件被 root 用户拥有
**文件所属用户组**：表示这个文件所属的用户组，这里表示 file 文件属于 root 用户组
**文件大小**：文件大小是 0 个字节
**文件修改时间**：这里的时间是该文件最后被更新（包括文件创建、内容更新、文件名更新等）的时间，可用命令 `stat [filename]` 查看文件的修改、访问、创建时间
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515173452661.png)

-------

## 文件索引节点 —— inode 

推荐文章：[学习篇 | LINUX 内核的文件系统 -- ext2](https://blog.csdn.net/weixin_42194161/article/details/104588023)

------



◆ 快速回顾 

@[toc](  )

◆ 其他博客 @ [https://blog.csdn.net/姜小逗](https://blog.csdn.net/weixin_42194161)

◆ 相关博客

- [学习篇 | LINUX 内核的文件系统 -- ext2](https://blog.csdn.net/weixin_42194161/article/details/104588023)



------

 感谢阅读本篇博客，如果有不错的发现和建议，感谢私信或在评论区留言
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424165249825.jpeg#pic_center)