Shell编程

Shell 是 Linux 系统中的一个重要的层次，他是用户与系统交互作用的界面

Shell 除了作为命令解释程序外，还是一种高级程序设计语言。利用 Shell 程序设计语言可以编写出功能很强，但代码简单的程序



Shell 基础介绍

下面介绍 Shell 的基础知识点

Shell 的定义：

Shell 既是一种命令语言，又是一种程序设计语言。

Shell 本身是用 C 语言编写的程序，它是用户使用 Linux 的桥梁

Shell 是一个命令语言解释器，它拥有自己内建的 Shell 命令集，Shell 也能被系统中其他应用程序所调用。

Shell 命令有内部命令和外部命令两种形式。



Shell 的分类

bash ksh ash csh zsh 等，使用不同的 Shell 的原因在于它们都有各自的特点。



bash

bash 是 Linux 系统默认使用的 Shell，它由 Brian Fox 和 Chet Ramey 共同完成，内部命令共有40个



ksh

ksh 是 Korm Shell 的缩写，共有 42 条内部命令。该 Shell 最大的优点是几乎和商业发行版的 ksh 完全相容



ash

ash Shell 是由 Kenneth Almquist 编写的，是 Linux 中占用系统资源最少的一个小 Shell，它只包含 24 个内部指令，因而使用起来很不方便



csh

csh 是 Linux 比较大的内核，它由以 William Joy 为代表的共计 47 位作者编成，共有 52 个内部指令。它其实是指向/bin/tcsh 的一个 Shell。csh 包括命令行编译、可编程单词补全、拼写校正、历史命令替换、作业控制和类似 C 语言的语法



zch 

zch 是 Linux 最大的 Shell 之一，由 Paul Falstad 完成，共有 84 个内部命令（但一般没必要安装使用）



Shell 启动过程

Linux 系统 Shell 的启动过程如下：

1）内核将加载至内存，直至系统关机。

2）init 将扫描 /etc/inittab，一旦找到活动的终端，mingetty 会给出 login 提示符和口令，mingetty 提示输入用户及口令

3）将用户名及口令传递给 login，login 验证用户及口令是否匹配，如果身份验证通过，login 将会自动转到其$HOME。

4）将控制权移交到所启动的任务。如在/etc.passwd 文件中用户的 Shell 为 /bin/sh

5）读取文件 /etc/profile 和 $HOME/.profile 中系统定义变量和用户定义变量，系统给出 Shell 提示符 $PROMPT，对普通用户用 $ 作提示符，对 root 用户用 # 作提示符

6）在 Shell 提示符，就可以输入命令名称及所需要的参数。Shell 将执行这些命令

7）当用户准备结束登录对话框进程时，可以键入 logout 命令，ext 命令或按 Ctrl+D，结束后控制权将交给 init



Shell 特殊字符

1. 单引号：消除被括在单引号中的所有特殊字符的含义

例如：

$ g=‘test’

$ echo “this is ‘$g’ ”

this is ‘test’

$ echo ‘$g’

$ g

可见，$保持了其本身的含义，作为普通字符出现



2. 双引号(??????反引号 书本错了？)

反引号用于设置系统命令的输出到变量。Shell 将反引号中的内容作为一个系统命令，并执行其内容。使用这种方法可以替换输出为一个变量。反引号可以与双引号结合使用。

例如：

$ echo “today is ‘date’”

today is Jul 7 19: 30: 35 CST 2009

$ echo “today is ‘date’”

today is date



3. 反斜线

如果下一个字符有特殊含义，反斜线防止 Shell 误解其含义，即屏蔽其特殊含义。下述字符包含有特殊意义：&、*、+、^、$、‘ ’、“ ”、|、?.

假如 echo 命令加 *，意即以串行顺序打印当前整个目录列表，而不是一个星号 *。为屏蔽星号特定含义，可使用反斜线。

例如：

$ echo \*

*



4. 注释符

在 Shell 编程或 Linux 的配置文档中，经常要对某些正文行进行注释，以增加程序的可读性。在 Shell 中以字符 # 开头的正文行表示注释行。





