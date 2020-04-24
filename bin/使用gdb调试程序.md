-----

用 GDB 调试程序





GDB 功能：

- 启动你的程序，可以按照你的自定义的要求随心所欲的运行程序。 
- 可让被调试的程序在你所指定的调置的断点处停住。（断点可以是条件表达式） 
- 当程序被停住时，可以检查此时你的程序中所发生的事。 
- 动态的改变你程序的执行环境。 



测试程序：

![1584152635824](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1584152635824.png)



![](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1584153075216.png)



源程序：

````
#include <stdio.h>

int func(int n)
{
    int sum = 0;
    for(int i = 0; i < n; i++)
    {
        sum += i;
    }
    return sum;
}

int main()
{
    int result = 0;
    for(int i = 0; i <= 100; i++)
    {
        result += i;
    }
    printf("result[1-100] = %d\n", result);
    printf("result[1-250] = %d\n", func(250));
    return 0;
}
````

注意 gcc 时，需要加 -g 选项，gcc -g [源文件] -o [生成的可执行程序]

使用 gdb 调试：

gdb [可执行程序名]，启动调试

`GNU gdb 5.1.1 
Copyright 2002 Free Software Foundation, Inc. 
GDB is free software, covered by the GNU General Public License, and you are welcome to change it and/or distribute copies of it under certain conditions. Type "show copying" to see the conditions. 
There is absolutely no warranty for GDB. Type "show warranty" for details. This GDB was configured as "i386-suse-linux"... `

先是出现上面的一些gdb的介绍，然后





------

注：

gcc -g 的含义：



gdb 时。list没有从第一行开始显示怎么办？————————》解决办法https://blog.csdn.net/qq_34501940/article/details/51920769



