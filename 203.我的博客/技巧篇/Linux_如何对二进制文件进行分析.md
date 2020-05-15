## 前言

Linux 下共有七种文件类型，普通文件类型是七种文件类型中最常见也是最多的一种。二进制文件是普通文件类型中的一份子。其实，我们天天在和二进制文件打交道，但很少有人知道它们的工作原理。

> 在 Linux 下，一切皆文件!!!

本篇博客意在讲解在 Linux 中用来分析`二进制文件`的常用指令

------

@[toc]( )

------

[Linux 下共有七种文件类型](https://blog.csdn.net/weixin_42194161/article/details/106140437)，普通文件类型是七种文件类型中最常见也是最多的一种。二进制文件是`普通文件类型`中的一份子。其实，我们天天在和二进制文件打交道，举个例子，你天天要使用的 Linux 命令，也是二进制文件的一种，但很少有人知道它们的工作原理。linux 中的二进制文件，一般以 `.o`, `.so`, `.a`, `.la` 后缀
> Linux下文件的类型是不依赖于其后缀名的，但一般来讲：
> .o 后缀是：目标文件,相当于windows中的.obj文件
> .so 后缀是：共享库,是shared object,用于动态连接的,和dll差不多
> .a 后缀是：静态库,是好多个.o合在一起,用于静态连接
> .la 后缀是：libtool自动生成的一些共享库，可用 vi 编辑查看，主要记录了一些配置信息。

推荐文章：[动态链接库(.so)以及静态链接库(.a)的生成和使用](https://blog.csdn.net/weixin_42194161/article/details/106142361)

------

## 二进制文件分析指令及工具
> `file`  `ldd`  `ltrace`  `strace`  `hexdump`  `strings`  `readelf`  `objdump`  `nm`  `gdb`
### file —— 用于分析文件的类型
如果你需要分析二进制文件，可以首先使用 file 命令来切入。我们知道，在 Linux 下，一切皆文件，但并不是所有的文件都具有可执行性，我们还有各种各样的文件，比如：文本文件，管道文件，链接文件，socket文件，等等。

在对一个文件进行分析之前，我们可以首先使用 file 命令来分析它们的类型。当然除此之外，我们还可以看到一些其它信息。

~~~bash
$ file /bin/pwd
/bin/pwd: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=f3c2ec3ab4ede5ab4588db69e8c1cfdb188b7ae7, stripped
~~~

-----

### ldd —— 用于分析可执行文件的依赖

我们使用 file 命令来分析一个可执行文件的时候，有时候可以看到输出中有 dynamically linked 这样的字眼。这个是啥意思呢？

大部分程序，都会使用到第三方库，这样就可以不用重复造轮子，节约大量时间。最简单的，我们写C程序代码的话，肯定会使用到 libc 或者 glibc 库。当然，除此之外，还可能使用其它的库。

那我们在什么情况下需要分析程序的依赖库呢？有一个场景大家肯定经历过。你去你同事那边拷备他写好的程序放到自己的环境下运行，有时候可能会跑不起来。当然跑不起来的原因可能很多，但其中一个原因可能就是缺少对应的依赖库。

这时候，ldd 就派上用场了。它可以分析程序需要一些什么依赖库，你只要把对应的库放在对应的位置就可以了。

~~~bash
$ ldd /bin/pwd
	linux-vdso.so.1 =>  (0x00007ffef1ddb000)
	libc.so.6 => /lib64/libc.so.6 (0x00007f490423a000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f490461e000)
~~~

------

### ltrace —— 能够跟踪进程的库函数调用
> 编译工具 ltrace 如果没有安装，使用 ltrace 指令会显示：
> `bash: ltrace: 未找到命令...`
> 安装 ltrace：
> `sudo yum install -y ltrace`
> 再输入密码，即可



我们可以使用 ldd 命令来找到程序的依赖库，但是，一个库里少则几个，多则几千个函数，怎么知道现在程序调用的是什么函数呢？

ltrace 命令就是用来做这个事的。在下面的例子里，我们可以看到程序调用的函数，以及传递进去的参数，同时你也可以看到函数调用的输出。

~~~bash
$ ltrace /bin/pwd
__libc_start_main(0x401760, 1, 0x7ffd63b06418, 0x404a00 <unfinished ...>
getenv("POSIXLY_CORRECT")                                = nil
strrchr("/bin/pwd", '/')                                 = "/pwd"
setlocale(LC_ALL, "")                                    = "zh_CN.UTF-8"
bindtextdomain("coreutils", "/usr/share/locale")         = "/usr/share/locale"
textdomain("coreutils")                                  = "coreutils"
__cxa_atexit(0x4022f0, 0, 0, 0x736c6974756572)           = 0
getopt_long(1, 0x7ffd63b06418, "LP", 0x606d00, nil)      = -1
getcwd(nil, 0)                                           = ""
puts("/home/JiangYu"/home/JiangYu
)                                    = 14
free(0x23fb040)                                          = <void>
exit(0 <unfinished ...>
__fpending(0x7fdddcbe0400, 0, 64, 0x7fdddcbe0eb0)        = 0
fileno(0x7fdddcbe0400)                                   = 1
__freading(0x7fdddcbe0400, 0, 64, 0x7fdddcbe0eb0)        = 0
__freading(0x7fdddcbe0400, 0, 2052, 0x7fdddcbe0eb0)      = 0
fflush(0x7fdddcbe0400)                                   = 0
fclose(0x7fdddcbe0400)                                   = 0
__fpending(0x7fdddcbe01c0, 0, 3328, 0xfbad000c)          = 0
fileno(0x7fdddcbe01c0)                                   = 2
__freading(0x7fdddcbe01c0, 0, 3328, 0xfbad000c)          = 0
__freading(0x7fdddcbe01c0, 0, 4, 0xfbad000c)             = 0
fflush(0x7fdddcbe01c0)                                   = 0
fclose(0x7fdddcbe01c0)                                   = 0
+++ exited (status 0) +++
~~~

------

### strace —— 用于追踪程序运行过程中的系统调用及信号
> 编译工具 strace 如果没有安装，使用 ltrace 指令会显示：
> `bash: strace: 未找到命令...`
> 安装 strace：
> `sudo yum install -y strace`
> 再输入密码，即可


通过上面的介绍，我们知道 ltrace 命令是用来追踪函数调用的。strace 命令类似，但它追踪的是系统调用。何为系统调用？简单说就是我们可以通过系统调用与内核进行交互，完成我们想要的任务。

例如，如果我们想在屏幕上打印某些字符，可以使用 printf 或 puts 函数，而这两个都是 libc 的库函数，在更底层，他们都是调用 write 这个系统调用。

~~~bash
$ strace -f /bin/pwd
execve("/bin/pwd", ["/bin/pwd"], [/* 24 vars */]) = 0
brk(NULL)                               = 0xbc9000
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f918ba69000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=38684, ...}) = 0
mmap(NULL, 38684, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f918ba5f000
close(3)                                = 0
open("/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\20&\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=2156160, ...}) = 0
mmap(NULL, 3985888, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f918b47b000
mprotect(0x7f918b63e000, 2097152, PROT_NONE) = 0
mmap(0x7f918b83e000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1c3000) = 0x7f918b83e000
mmap(0x7f918b844000, 16864, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f918b844000
close(3) 
…………
+++ exited with 0 +++
~~~

------

### hexdump —— 查看二进制文件的 16 进制编码

一个二进制文件，如果你直接使用文本编辑器打开的话，将看到一堆乱码。这时候，你就可以使用 hexdump 命令来查看它的内容了。

hexdump 的显示格式是：左边是字节序号，中间是文件的 16 进制编码，如果是可打印字符的话就会显示在右边。

通过使用这个命令，我们就可以大概知道这个二进制文件里面有什么内容，后面要做什么处理就比较方便了。

~~~bash
$ hexdump -C /bin/pwd | head
00000000  7f 45 4c 46 02 01 01 00  00 00 00 00 00 00 00 00  |.ELF............|
00000010  02 00 3e 00 01 00 00 00  17 19 40 00 00 00 00 00  |..>.......@.....|
00000020  40 00 00 00 00 00 00 00  50 7a 00 00 00 00 00 00  |@.......Pz......|
00000030  00 00 00 00 40 00 38 00  09 00 40 00 1e 00 1d 00  |....@.8...@.....|
00000040  06 00 00 00 05 00 00 00  40 00 00 00 00 00 00 00  |........@.......|
00000050  40 00 40 00 00 00 00 00  40 00 40 00 00 00 00 00  |@.@.....@.@.....|
00000060  f8 01 00 00 00 00 00 00  f8 01 00 00 00 00 00 00  |................|
00000070  08 00 00 00 00 00 00 00  03 00 00 00 04 00 00 00  |................|
00000080  38 02 00 00 00 00 00 00  38 02 40 00 00 00 00 00  |8.......8.@.....|
00000090  38 02 40 00 00 00 00 00  1c 00 00 00 00 00 00 00  |8.@.............|
~~~
**注：实际上 hexdump 命令可以用来查看任何文件，而不限于二进制文件**。

------

### strings —— 用来打印二进制文件中可显示的字符

什么是可显示字符？简单说你在显示器上看到的字符都是可显示字符，比如：abcABC,.:。

我们知道，一个二进制文件里面的内容很多是非显示字符，所以无法直接用文本处理器打开。程序在被开发的时候，我们经常会加一些调试信息，比如：debug log, warn log, error log，等等。这些信息我们就可以使用 strings 命令看得到。

~~~bash
$ strings /bin/pwd | head
/lib64/ld-linux-x86-64.so.2
libc.so.6
fflush
strcpy
__printf_chk
readdir
setlocale
mbrtowc
strncmp
optind
~~~

------

### readelf —— 查看 ELF 格式的文件信息

ELF（Executable and Linkable Format）即可执行连接文件格式，是一种比较复杂的文件格式，但其应用广泛。当你使用 file 命令发现某个文件是 ELF 文件时，你就可以使用 readelf 命令来读取这个文件的信息。

~~~bash
$ readelf -h /bin/pwd
ELF 头：
  Magic：  7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (可执行文件)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  入口点地址：              0x401917
  程序头起点：              64 (bytes into file)
  Start of section headers:          31312 (bytes into file)
  标志：             0x0
  本头的大小：       64 (字节)
  程序头大小：       56 (字节)
  Number of program headers:         9
  节头大小：         64 (字节)
  节头数量：         30
  字符串表索引节头： 29
~~~

------

### objdump —— 查看目标文件或者可执行的目标文件的构成
**objdump是用查看目标文件或者可执行的目标文件的构成的GCC工具**。

我们知道，程序在开发完成之后，需要经过编译，才可以生成计算机可以识别的二进制文件。我们写的代码计算机不能直接执行，需要编译成汇编程序，计算机才能依次执行。

objdump 命令可以读取可执行文件，然后将汇编指令打印出来。所以如果你想看懂 objdump 的结果，你就需要有一些汇编基础才可以。

~~~bash
$ objdump -d /bin/pwd | head

/bin/pwd：     文件格式 elf64-x86-64


Disassembly of section .init:

0000000000401350 <.init>:
  401350:	48 83 ec 08          	sub    $0x8,%rsp
  401354:	48 8b 05 6d 5c 20 00 	mov    0x205c6d(%rip),%rax        # 606fc8 <__ctype_b_loc@plt+0x205878>
  40135b:	48 85 c0             	test   %rax,%rax
~~~

------

### nm —— 列出目标文件的符号 (函数和全局变量)

如果你编译出来的程序没有经过 strip ，那么 nm 命令可以挖掘出隐含在可执行文件中的重大秘密。它可以帮你列出文件中的变量及函数，这对于我们进行反向操作具有重大意义。

下面我们通过一小段简单的程序来讲解 nm 命令的用途。在编译这个程序时，我们加上了 -g 选项，这个选项可以使编译出来的文件包含更多有效信息。

~~~bash
$ cat hello.c 
#include <stdio.h>

int main() {
    printf("Hello world!\n");
    return 0;
}
$ 
$ gcc -g hello.c -o hello
$ 
$ file hello
hello: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=3de46c8efb98bce4ad525d3328121568ba3d8a5d, not stripped
$ 
$ ./hello 
Hello world!
$ 
$ 
$ nm hello | tail
0000000000600e20 d __JCR_END__
0000000000600e20 d __JCR_LIST__
00000000004005b0 T __libc_csu_fini
0000000000400540 T __libc_csu_init
                 U __libc_start_main@@GLIBC_2.2.5
000000000040051d T main
                 U printf@@GLIBC_2.2.5
0000000000400490 t register_tm_clones
0000000000400430 T _start
0000000000601030 D __TMC_END__
$
~~~

------

### gdb —— GNU debugger

gdb 大家或多或少都有听说过。我们在使用一些 IDE 写代码的时候，可以进行打断点、步进、查看变量值等方式调试，其实这些 IDE 底层调用的也是 gdb 。

对于 gdb 的用法，可以写很多，本文就暂且不深入了。下面先演示一小段 gdb 最基础的功能。


~~~bash
$ gdb -q ./hello
Reading symbols from /home/flash/hello...done.
(gdb) break main
Breakpoint 1 at 0x400521: file hello.c, line 4.
(gdb) info break
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x0000000000400521 in main at hello.c:4
(gdb) run
Starting program: /home/flash/./hello 

Breakpoint 1, main () at hello.c:4
4           printf("Hello world!");
Missing separate debuginfos, use: debuginfo-install glibc-2.17-260.el7_6.6.x86_64
(gdb) bt
#0  main () at hello.c:4
(gdb) c
Continuing.
Hello world![Inferior 1 (process 29620) exited normally]
(gdb) q
$
~~~

-----

## 应用说明
当你在 linux 下拿到一个二进制文件但不知道它是什么的时候,可以通过以下方法得到一此提示

（一）首先应该尝试 `strings` 命令
比如拿到一个叫 cr1 的二进制文件,可以:
~~~bash
$ strings cr1 | more
~~~

显示的内容里可能会有一些对于这个 cr1 的描述，这些信息都是编译之后在程序中留下的一些文本性的说明，所以可能会告诉你这个文件是什么。

比如有输出:
~~~bash
$ strings cr1 | more
...
Version: 2.3
Usage: dsniff [-cdmn] [-i interface] [-s snaplen] [-f services]
[-t trigger[,...]] [-r|-w savefile] [expression]
...
/usr/local/lib/dsniff.magic
/usr/local/lib/dsniff.services
...
~~~
那么我们就可以知道，其实 cr1 就是 dsniff 命令.

（二）如果第一步没有帮助，那么你接着可以尝试 `nm` 
~~~bash
$ /usr/ccs/bin/nm -p cr1 | more
~~~

比如说可以得到如下输出：
~~~bash
cr1:
　　[Index]   Value    Size Type    Bind Other   Shndx       Name
　　[180]     |0     |    0| FILE | LOCL | 0     |ABS |       decode_smtp.c
　　[2198]    |160348| 320| FUNC | GLOB | 0     | 9 |       decode_sniffer
~~~
这些都是生成这个二进制文件的 obj 文件的文件名称，这些名称会告诉你这个二进制文件的作用。

同样，如果希望查看二进制文件调用到的静态库文件都有哪些的话，可以使用 `nm -Du cr1` 来实现.

（三）当然还可以通过使用 `dump` 命令来得到任何一个二进制文件的选定部分信息
~~~bash
$ /usr/ccs/bin/dump -c ./cr1 | more
~~~
dump命令的参数说明:
~~~C
-c		Dump出字符串表
-C		Dump出C++符号表
-D		Dump出调试信息
-f 		Dump出每个文件的头
-h 		Dump出section的头
-l 		Dump出行号信息
-L 		Dump出动态与静态链接库部分内容
-o		Dump出每个程序的可执行头
-r		Dump出重定位信息
-s 		用十六进制信息Dump出section的内容
-t 		Dump符号表.
~~~
（四）使用 `file` 命令得到二进制文件的信息
~~~bash
$ file cr1
~~~
（五）如果还是不清楚的话,那么我们可以使用 `ldd` 命令
~~~bash
$ ldd cr1
~~~
比如说输出为:
~~~bash
	...
	libsocket.so.1 =>       /usr/lib/libsocket.so.1
	librpcsvc.so.1 =>       /usr/lib/librpcsvc.so.1
	...
~~~

那么我们就可以知道这个程序与网络库相关,我们就可以知道它的大概功能了.

（六）我们也可以能过 `adb` 命令来得到一个二进制文件的执行过程
比如说:
~~~bash
$ adb cr1
:r
Using device /dev/hme0 (promiscuous mode)
192.168.2.119 -> web      TCP D=22 S=1111 Ack=2013255208
Seq=1407308568 Len=0 Win=17520
web -> 192.168.2.119 TCP D=1111 S=22 Push Ack=1407308568
~~~
我们知道这个程序是一个sniffer.

（七）如果你确定要运行这个程序的话，你可以先通过 `truss` 命令打开系统的信号与调用输出：
~~~bash
$ truss -f -o cr.out ./cr1
listening on hme0
^C
$
~~~
这样就可以知道这个程序到底干了什么

----

**小结**
有了上面这些工具的话，我们就可以大概了解到一个未知的二进制程序到底是干什么的。但最后提示大家：**运行不了解的二进制程序有严重的安全问题**，请大家千万小心！！！

------



◆ 快速回顾 

@[toc](  )

◆ 其他博客 @ [https://blog.csdn.net/姜小逗](https://blog.csdn.net/weixin_42194161)

◆ 相关博客

- [学习篇  |  Linux环境中 -- 静态库&动态库的生成与使用](https://blog.csdn.net/weixin_42194161/article/details/106142361)
- [学习篇 | Linux_文件类型_文件属性_文件inode](https://blog.csdn.net/weixin_42194161/article/details/106140437)



------

 感谢阅读本篇博客，如果有不错的发现和建议，感谢私信或在评论区留言
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424165249825.jpeg#pic_center)