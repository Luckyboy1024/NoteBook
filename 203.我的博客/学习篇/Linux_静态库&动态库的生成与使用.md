### 前言


在我们编写代码的时候，经常有用到一些库的接口，这些库有两种常见形式，一种以 .a(.lib) 为后缀，为静态库；另一种以 .so(.dll) 为后缀，为动态库。那么这两种库有什么区别呢？这两种库又是如何被生成和使用的呢？

------



@[toc]( )

------

**内容整理：**

 > 适用环境：Linux 系统环境

 - 动态库的生成和使用
	- 生成：gcc -shared -fPIC test.c -o libtest.so
	- 使用：gcc main.c -o out -L ./ -ltest 
	- ldd 查看所依赖的库文件
	- 设置 LD_LIBRARY_PATH 环境变量
- 静态库的生成和使用
	- 生成：ar -rc lib[filename].a [file].o
	- 查看静态库中的目录列表：ar -tv libmymath.a
	- 使用：gcc main.c -o out -L ./ -lmymath

----

### 两种库的区别

- 动态库（.so）：程序在运行阶段会依赖操作系统加载的so文件，从而完成任务
- 静态库（.a）：程序在编译的阶段，依赖的函数统统都会编译到可执行程序当中去，当程序要执行的时候，不需要依赖静态库

### 动态库的生成与使用
我们新建一个目录文件来进行演示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301155810676.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301155821560.png)
创建两个文件，以 test.c test.h 为例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301155839922.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)
此时，当前目录底下有两个文件：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301155855659.png)
注意：这两个文件中都是没有 main 函数的，因为要生成动态库中，库函数是不能有main函数的

接下来，就是生成动态库了：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301155919243.png)


- [-shared]：产生动态库，产生的动态so可共享
- [-fPIC]：产生与位置无关的代码，使函数调用时使用的是相对地址
- [test.c]：编译的目标文件(或文件的路径)
- [-o]：使其生成的是一个可执行程序或库文件，此时我们是要它生成一个库文件
- [libtest.so]：命名有要求：必须是“lib***.so”，文件名以 lib 开始，以 .so 结束，中间的 *** 是库名称


接下来，我们写一个测试用的 .c 文件，以 main.c 为例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301155944721.png)
然而，当我们编译这个main.c文件时，报错了：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160002210.png)
什么问题？编译时，要声明一下它所依赖的库，如何声明：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160014799.png)
找不到我们的test库，是因为我们忘了设置当前这个库的路径
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160041164.png)
回车：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160054660.png)
运行：` ./out`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160117677.png)
如何查看，我们可执行程序依赖的是哪个动态库：ldd + [可执行程序名称]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160138875.png)
小结：
> 生成：gcc -shared -fPIC test.c -o libtest.so
> 使用：gcc main.c -o out -L ./ -ltest 
> 查看：ldd out

如果我们移动动态库的位置，再运行程序会报错
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160244205.png)
移回来：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160254417.png)
那如果我想把它（动态库）放在其他目录里面，怎么办呢？
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160311703.png)
敲回车：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160320550.png)
设置环境变量：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160333865.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)
如何使这个环境变量一直生效 (通过“指令”在任何位置直接启动我们的程序)？

- 先任选以下文件路径中的任意一个
	- /etc/bashrc
	- ~/.bashrc
	- ~/.bash_profile
- 使用 vi 编辑器对这个文件进行编辑更改：添加到其中的环境变量，保存退出；
- 然后输入指令 source [filename] filename为刚选择的文件路径，使指定环境变量文件生效，(重新加载终端) 即可实现
- 设置完环境变量，我们就可以在正常使用我们的动态库了

out 执行到 libtest.so 的库函数print()时，会去看环境变量，在环境变量显示的路径下找到动态库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160357218.png)
此时，再 ldd 就可以看到它显示了我们动态库的地址

-----

### 静态库的生成与使用
测试程序：

```
/////////////add.h/////////////////     
#ifndef __ADD_H__
#define __ADD_H__
int add(int a, int b);
#endif // __ADD_H__
/////////////add.c/////////////////     
#include "add.h"
int add(int a, int b)
{
    return a + b;
}
/////////////sub.h/////////////////
#ifndef __SUB_H__
#define __SUB_H__
int sub(int a, int b);
#endif // __SUB_H__
/////////////sub.c/////////////////     
#include "add.h"
int sub(int a, int b)
{
     return a - b;
}
///////////main.c////////////////     
#include <stdio.h>
#include "add.h"
#include "sub.h"
int main( void )
{
    int a = 10;
    int b = 20;
    printf("add(10, 20)=%d\n", a, b, add(a, b));
    a = 100;
    b = 20;
    printf("sub(%d,%d)=%d\n", a, b, sub(a, b));
}
```
在 vim 编辑器中，大概是这个样子：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160526759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160506420.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301160514324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20200301160526759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)
生成：ar -rc lib[filename].a [file].o
![](https://img-blog.csdnimg.cn/20200301160542248.png)
查看静态库中的目录列表：ar -tv libmymath.a ，选项 [-tv]:

- t :  列出静态库中的文件
- v :  verbose 详细信息
![](https://img-blog.csdnimg.cn/20200301160614243.png)
使用：链接静态库：gcc [源码文件] -o [生成的可执行程序] -L [path] -l[静态库的名称]，例：gcc main.c -L . lmymath
![](https://img-blog.csdnimg.cn/20200301160625613.png)
![](https://img-blog.csdnimg.cn/20200301160631215.png)
库搜索路径：
- 从左到右搜索-L指定的目录。
- 由环境变量指定的目录 （LIBRARY_PATH）
- 由系统指定的目录
	- /usr/lib
	- /usr/local/lib
------

### 应用场景
- 测试目标文件生成后，静态库删掉，程序照样可以运行。
- 在工作中，如果我们的可执行程序跑不起来，使用 ldd 看一下它依赖哪些库文件，如果程序找不到某一个库文件，ldd 后显示的库文件后显示的肯定是 "cannot find"





------



◆ 快速回顾 

@[toc](  )

◆ 其他博客 @ [https://blog.csdn.net/姜小逗](https://blog.csdn.net/weixin_42194161)

◆ 相关博客


- [学习篇 | LINUX 内核的文件系统 -- ext2](https://blog.csdn.net/weixin_42194161/article/details/104588023)
- [学习篇 | 浮点数的表示规则](https://blog.csdn.net/weixin_42194161/article/details/99056741)



------

 感谢阅读本篇博客，如果有不错的发现和建议，感谢私信或在评论区留言
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424165249825.jpeg#pic_center)