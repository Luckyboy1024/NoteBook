## 前言 

本篇博客讲述的是如何利用C语言做一个计算器，简单实现生活中常用的乘除加减，有两个版本，对比分析，判断此处使用函数指针来实现的优劣性

------



@[toc]



------


### 非函数指针版 —— 计算器

#### 代码示例
~~~
#include <stdio.h>
int add(int a, int b)
{
   return a + b;
}
int sub(int a, int b)
{
   return a - b;
}
int mul(int a, int b)
{
   return a*b;
}
int div(int a, int b)
{
   return a / b;
}
int main()
{
   int x, y;
   int input = 1;
   int ret = 0;
   while (input)
   {
       printf("*************************\n");
       printf("  1:add           2:sub   \n");
       printf("  3:mul           4:div   \n");
       printf("*************************\n");
       printf("请选择：");
       scanf("%d", &input);
       switch (input)
       {
       case 1:
          printf("输入操作数：");
          scanf("%d %d", &x, &y);
          ret = add(x, y);
          break;
       case 2:
          printf("输入操作数：");
          scanf("%d %d", &x, &y);
          ret = sub(x, y);
          break;
       case 3:
          printf("输入操作数：");
          scanf("%d %d", &x, &y);
          ret = mul(x, y);
          break;
       case 4:
          printf("输入操作数：");
          scanf("%d %d", &x, &y);
          ret = div(x, y);
          break;
       default:
          printf("选择错误\n");
          break;
       }
       printf("ret = %d\n", ret);
   }
   return 0;
}
~~~

#### 运行示例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200508203949525.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)

----

### 函数指针版 —— 计算器 PLus

#### 代码示例

~~~
#include <stdio.h>
int add(int a, int b)
{
   return a + b;
}
int sub(int a, int b)
{
   return a - b;
}
int mul(int a, int b)
{
   return a*b;
}
int div(int a, int b)
{
   return a / b;
}
int main()
{
   int x, y;
   int input = 1;
   int ret = 0;
   int(*p[5])(int x, int y) = { 0, add,  sub, mul, div }; //转移表
   while (input) {
       printf(  "*************************\n" );
       printf( "  1:add           2:sub   \n" );
       printf( "  3:mul           4:div   \n" );
       printf(  "*************************\n" );
       printf( "请选择：" );
       scanf( "%d", &input);
       if ((input <= 4 && input >= 1))
       {
          printf( "输入操作数：" );
          scanf( "%d %d", &x, &y);
          ret = (*p[input])(x, y);    //相当于ret = p[input](x, y)
       }
       else{
          printf( "输入有误\n" );
       }
       printf( "ret = %d\n", ret);
   }
   return 0;
}

~~~



#### 运行示例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200508204107356.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)
**小结：** 利用函数指针数组，通过下标访问数组来间接访问对应的函数


简单实现了乘除加减可能还是无法满足我们计算器的基本需要，偶尔还需要 **取模、开平方、累乘** 等等的计算，这里就不再赘述，请大家动手练习

------

这里留一个尾巴，下篇博客讲述：『linux C 函数库 —— 数学计算函数 』

-----




◆ 回到开头 @[目录]()

- [非函数指针版](https://blog.csdn.net/weixin_42194161/article/details/106005226#___14)
- [函数指针版](https://blog.csdn.net/weixin_42194161/article/details/106005226#___PLus_85)

◆ 其他博客 @ [https://blog.csdn.net/姜小逗](https://blog.csdn.net/weixin_42194161)

◆ 相关博客

- [探索篇 | C简单实现『字符动画』](https://blog.csdn.net/weixin_42194161/article/details/94343816)
- [探索篇 | C实现猜数字游戏](https://blog.csdn.net/weixin_42194161/article/details/95732303)



------

 感谢阅读本篇博客，如果有不错的发现和建议，感谢私信或在评论区留言
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424165249825.jpeg#pic_center)