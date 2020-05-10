## 前言 

本篇博客是对 `Linux_C 函数库`中用于 `数学计算` 函数（例如： `sqrt()`, `abs()`,`pow()`等函数）的总结，在此与大家分享

------


 @[toc]( )

------


## abs       ---- 计算整型数的绝对值

- 相关函数：labs, fabs
- 表头文件：#include <stdlib.h>
- 定义函数：int abs(int j);
- 函数说明：abs() 用来计算参数 j 的绝对值，然后将结果返回。
- 返回值：返回参数 j 的绝对值计算结果。

**代码范例：**
~~~
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int answer = 0;
    answer = abs(-12);
    printf(" | -12 | = %d\n", answer);           
    return 0;          
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509200142349.png)

------

## fabs      ---- 计算浮点型数的绝对值

- 相关函数：abs, labs
- 表头文件：#include <math.h>
- 定义函数：double fabs(double x);
- 函数说明：fabs() 用来计算浮点型数 x 的绝对值，然后将结果返回。
- 返回值：返回参数 x 的绝对值计算结果

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = 0.0;
    answer = fabs (-3.141592);
    printf("| -3.141592 | = %f\n", answer);  
    return 0;     
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509201557588.png)

------

## labs     ---- 计算长整型数的绝对值

- 相关函数：abs, fabs
- 表头文件：#include <stdlib.h>
- 定义函数：long int labs(long int j);
- 函数说明：labs() 用来计算参数 j 的绝对值，然后将结果返回。
- 返回值：返回参数 j 的绝对值计算结果
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>  
#include <stdlib.h>  
  
int main()  
{  
    long int answer = 0;  
    answer = labs (-2000);  
    printf("| -2000 | = %ld\n", answer);
    return 0;                 
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/202005092018266.png)

------

## ceil       ---- 取不小于参数的最小整型数

- 相关函数：fabs
- 表头文件：#include <math.h>
- 定义函数：double ceil(double x);
- 函数说明：ceil() 会返回不小于参数 x 的最小整数值，结果以 double 形态返回。
- 返回值：返回不小于参数 x 的最小整数值。
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()                                                                                
{
    double value[] = { 4.8, 1.12, -2.2, 0 };
    int i = 0;
    for (i = 0; value[i] != 0; i++)
    {
        printf("%f => %f\n", value[i], ceil(value[i]));
    }
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509201007924.png)

------


## exp       ---- 计算指数

- 相关函数：log, log10, pow
- 表头文件：#include <math.h>
- 定义函数：double exp(double x);
- 函数说明：exp() 用来计算以 e 为底的 x 次方值，即 ex 值，然后将结果返回。
- 返回值：返回 e 的 x 次方计算结果。
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = 0.0;
    answer = exp(10);
    printf("e^10 = %f\n", answer);        
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509201458278.png)

------

## ldexp   ---- 计算 2 的次方值

- 相关函数：frexp
- 表头文件：#include <math.h>
- 定义函数：double ldexp(double x, int exp);
- 函数说明：ldexp() 用来将参数 x 乘上 2 的 exp 次方值，即 x*2exp 
- 返回值：返回计算结果
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
/* 计算 3*(2^2) = 12 */
#include <stdio.h>  
#include <math.h>  
  
int main()  
{  
    int exp = 2;                                                                          
    double x = 3.0;  
    double answer = 0.0;  
    answer = ldexp(x, exp);  
    printf("3*2^(2) = %f\n", answer);    
    return 0;    
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509201909201.png)

------

## frexp    ---- 将浮点型数分为底数与指数

- 相关函数：ldexp, modf
- 表头文件：#include <math.h>
- 定义函数：double frexp(double x, int* exp);
- 函数说明：frexp() 用来将参数 x 的浮点型数切割成底数和指数。底数部分直接返回，指数部分则借参数 exp 指针返回，将返回值乘以 2 的 exp 次方即为 x 的值
- 返回值：返回参数 x 的底数部分，指数部分则存于 exp 指针所指的地址
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    int exp = 0;
    double fraction = 0.0;
    fraction = frexp(1024, &exp);
    printf("exp = %d\n", exp);
    printf("fraction = %f\n", fraction);
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509201642629.png)
注：0.5 * (2^11) = 1024

------

## log       ---- 计算以 e 为底的对数值

- 相关函数：exp, log10, pow
- 表头文件：#include <math.h>
- 定义函数：double log(double x);
- 函数说明：log() 用来计算以 e 为底的 x 对数值，然后将结果返回
- 返回值：返回参数 x 的自然对数值
- 错误码：
  - EDOM  参数 x 为负数
  - ERANGE    参数 x 为零值，零的对数值无定义
- 附加说明：使用 GCC 编译时请加入 -lm


**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = 0.0;
    answer = log(100);
    printf("log(100) = %f\n", answer);              
    return 0;                                         
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509202156215.png)

------

## log10   ---- 计算以 10 为底的对数值

- 相关函数：exp, log, pow
- 表头文件：#include <math.h>
- 定义函数：double log10(double x);
- 函数说明：log10() 用来计算以 10 为底的 x 对数值，然后将结果返回
- 返回值：返回参数 x 以 10 为底的对数值
- 错误码：
  - EDOM  参数 x 为负数
  - ERANGE    参数 x 为零值，零的对数值无定义
- 附加说明：使用 GCC 编译时请加入 -lm


**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = 0.0;
    answer = log10(100);
    printf("log10(100) = %f\n", answer);    
    return 0;                
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509202303167.png)

------

## modf   ---- 将浮点数分解成整数与小数

- 相关函数：frexp
- 表头文件：#include <math.h>
- 定义函数：double modf(double x, double* iptr);
- 函数说明：modf() 用来将参数 x 的浮点型数分解成整数和小数。小数部分直接返回，整数部分则借参数 iptr 指针返回
- 返回值：返回参数 x 的小数部分，整数部分则存于 iptr 指针所指的地址
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
/* 分解 3.14159 的整数和小数部分 */
#include <stdio.h>
#include <math.h>

int main()
{
    double integral = 0.0;
    double fractional = 0.0;
    fractional = modf(3.14159, &integral);
    printf("integral   = %f\n", integral);
    printf("fractional = %f\n", fractional);   
    return 0;            
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509202348264.png)

------

## pow    ---- 计算次方值

- 相关函数：exp, log, log10
- 表头文件：#include <math.h>
- 定义函数：double pow(double x, double y);
- 函数说明：pow() 用来计算以 x 为底的 y 次方值，即 x,y 值，然后将结果返回
- 返回值：返回 x 的 y 次方计算结果
- 错误码：EDOM 参数 x 为负数且参数 y 不是整数
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = 0.0;
    answer = pow(2, 10);
    printf("2^10 = %f\n", answer);
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509202429631.png)

------


## sqrt    ---- 计算平方根值

- 相关函数：hypotq
- 表头文件：#include <math.h>
- 定义函数：double sqrt(double x);
- 函数说明：sqrt() 用来计算参数 x 的平方根，然后将结果返回。参数 x 必须为正数
- 返回值：返回参数 x 的平方根值
- 错误码：EDOM   参数 x 为负值
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
/* 计算 200 的平方根值 */
#include <stdio.h>
#include <math.h>

int main()
{
    double root = 0.0;
    root = sqrt(200);
    printf("answer is %f\n", root);                                   
    return 0;             
} 
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509202637319.png)

------


## hypot   ---- 计算直角三角形斜边长

- 相关函数：sqrt
- 表头文件：#include <math.h>
- 定义函数：double hypot(double x, double y);
- 函数说明：hypot() 是调用 sqrt(x*x + y*y) 然后将计算结果返回。通常用来计算直角三角形斜边长，参数 x 和 y 为两边边长，或是计算原点到点 (x , y) 的距离
- 返回值：返回 (x*x + y*y) 的平方根值
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
/* 计算点 (3, 4) 至原点的距离 */
#include <stdio.h>
#include <math.h>

int main()
{
    double distance = 0.0;
    distance = hypot(3, 4);
    printf("distance of the point (3, 4) from the origin is %f\n", distance);
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509201744570.png)

------

## asin      ---- 取反正弦函数值

- 相关函数：acos, atan, atan2, cos, sin, tan
- 表头文件：#include <math.h>
- 定义函数：double asin(double x);
- 函数说明：asin() 用来计算参数 x 的反正弦值，然后将结果返回。参数 x 范围为 -1 至 1 之间，超过此范围则会失败。
- 返回值：返回 -PI/2 至 PI/2 之间的计算结果，单位为弧度，在函数库中角度均以弧度来表示。
- 错误码：EDOM 参数 x 超出范围。
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>  
#include <math.h>  

int main()
{
    double angle = 0.0;
    angle = asin(0.5);
    printf("angle = %f\n", angle);
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509200626861.png)

------


## acos     ----  取反余弦函数值

- 相关函数：asin, atan, atan2, cos, sin, tan
- 表头文件：#include <math.h>
- 定义函数：double acos(double x);
- 函数说明：acos() 用来计算参数 x 的反余弦值，然后将结果返回。参数 x 范围为 -1 至 1 之间，超过此范围则会失败。
- 返回值：返回 0 至 PI 之间的计算结果，单位为弧度，在函数库中角度均以弧度来表示。
- 错误码：EDOM 参数 x 超出范围。
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double angle = 0.0;
    angle = acos(0.5);
    printf("angle = %f\n", angle);
    return 0;          
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509200501818.png)

------



## atan      ---- 取反正切函数值

- 相关函数：acos, asin, atan2, cos, sin, tan
- 表头文件：#include <math.h>
- 定义函数：double atan(double x);
- 函数说明：atan() 用来计算参数 x 的反正切值，然后将结果返回。
- 返回值：返回 -PI/2 至 PI/2 之间的计算结果，单位为弧度，在函数库中角度均以弧度来表示
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double angle = 0.0;
    angle = atan(1);
    printf("angle = %f\n", angle);
    return 0;    
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509200723861.png)

------

## atan2    ---- 取得反正切函数值

- 相关函数：acos, asin, atan, cos, sin, tan
- 表头文件：#include <math.h>
- 定义函数：double atan2(double y, double x);
- 函数说明：atan2() 用来计算参数 y/x 的反正切值，然后将结果返回。
- 返回值：返回 -PI/2 至 PI/2 之间的计算结果，单位为弧度，在函数库中角度均以弧度来表示。
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double angle = 0.0;
    angle = atan2(1, 2);
    printf("angle = %f\n", angle);
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509200910619.png)

------


## sin      ---- 取正弦函数值

- 相关函数：acos, asin, atan, atan2, cos, tan 
- 表头文件：#include <math.h>
- 定义函数：double sin(double x);
- 函数说明：sin() 用来计算参数 x 的正弦值，然后将结果返回
- 返回值：返回 -1 至 1 之间的计算结果
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = sin(0.5);
    printf("sin(0.5) = %f\n", answer);                                                    
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020050920251321.png)

------

## cos       ---- 取余弦函数值

- 相关函数：acos, asin, atan, atan2, sin, tan
- 表头文件：#include <math.h>
- 定义函数：double cos(double x);
- 函数说明：cos() 用来计算参数 x 的余弦值，然后将结果返回。
- 返回值：返回 -1 至 1 之间的计算结果。
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = cos(0.5);
    printf("cos [0.5] = %f\n", answer);
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509201053282.png)

------

## tan     ---- 取正切函数值

- 相关函数：atan, atan2, cos, sin
- 表头文件：#include <math.h>
- 定义函数：double tan(double x);
- 函数说明：tan() 用来计算参数 x 的正切值，然后将结果返回
- 返回值：返回参数 x 的正切值
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = tan(0.5);
    printf("tan(0.5) = %f\n", answer);             
    return 0;                
}  
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509202710896.png)

------

## sinh    ---- 取双曲线正弦函数值

- 相关函数：cosh, tanh
- 表头文件：#include <math.h>
- 定义函数：double sinh(double x);
- 函数说明：sinh() 用来将参数 x 的双曲线正弦值，然后将结果返回。数学定义式为：(exp(x)-exp(-x))/2
- 返回值：返回参数 x 的双曲线正弦值
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = sinh(0.5);
    printf("sinh(0.5) = %f\n", answer);                        
    return 0;                 
}    
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509202550822.png)

------

## cosh     ---- 取双曲线余弦函数值

- 相关函数：sinh, tanh
- 表头文件：#include <math.h>
- 定义函数：double cosh(double x);
- 函数说明：cosh() 用来计算参数 x 的双曲线余弦值，然后将结果返回。数学定义式为：(exp(x) + exp(-x)) / 2
- 返回值：返回参数 x 的双曲线余弦值
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = cosh(0.5);
    printf("cosh (0.5) = %f\n", answer);
    return 0;
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509201152107.png)

------

## tanh   ---- 取双曲线正切函数值

- 相关函数：cosh, sinh
- 表头文件：#include <math.h>
- 定义函数：double tanh(double x);
- 函数说明：tanh() 用来计算参数 x 的双曲线正切值，然后将结果返回。数学定义式为：sinh(x)/cosh(x)
- 返回值：返回参数 x 的双曲线正切值
- 附加说明：使用 GCC 编译时请加入 -lm

**代码范例：**
~~~
#include <stdio.h>
#include <math.h>

int main()
{
    double answer = tanh(0.5);
    printf("tanh(0.5) = %f\n", answer);           
    return 0;                                                                   
} 
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020050920275257.png)

------

## div        ---- 取得两整型数相除后的商及余数

- 相关函数：ldiv
- 表头文件：#include <stdlib.h>
- 定义函数：div_t div(int numerator, int denominator);
- 函数说明：div() 函数会计算参数 numerator / denominator，然后将相除后的商及余数由 div_t 结构返回。div_t 结构定义如下：

~~~ C
    typedef struct
    {
        int quot;   /* 商数 */
        int rem;    /* 余数 */
    } div_t;
~~~
- 返回值：返回 div_t 结构，包含商数及余数

**代码范例：**
~~~
#include <stdio.h>
#include <stdlib.h>

int main()
{
    div_t answer;
    answer = div(67, 4);
    printf("Quotient = %d, remainder = %d\n", answer.quot,answer.rem);    
    return 0;                                                                   
}  
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020050920141526.png)

------

## ldiv      ---- 取得两长整型数相除后的商及余数

- 相关函数：div
- 表头文件：#include <stdlib.h>
- 定义函数：ldiv_t ldiv(long int numerator, long int denominator);
- 函数说明：ldiv() 函数会计算参数 numerator / denominator，然后将相除后的商及余数由 ldiv_t 结构返回。div_t 结构定义如下：
~~~C
    typedef struct
    {
        long int quot;   /* 商数 */
        long int rem;    /* 余数 */
    } ldiv_t;
~~~
- 返回值：返回 ldiv_t 结构，包含商数及余数

**代码范例：**
~~~
/* 计算 2345678 / 76542 的商及余数 */
#include <stdio.h>  
#include <stdlib.h>  
  
int main()  
{  
    ldiv_t answer;  
    answer = ldiv(2345678, 76542);  
    printf("Quotient = %ld, remainder = %ld\n", answer.quot, answer.rem);    
    return 0;   
}
~~~

**执行结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509202034147.png)

------



◆ 快速回顾 
@[toc](  )


◆ 其他博客 @ [https://blog.csdn.net/姜小逗](https://blog.csdn.net/weixin_42194161)

◆ 相关博客

- [探索篇 | C 实现计算器](https://blog.csdn.net/weixin_42194161/article/details/106005226)
- [逻辑题 | 水仙花数与兰德尔数](https://blog.csdn.net/weixin_42194161/article/details/98644070)



------

 感谢阅读本篇博客，如果有不错的发现和建议，感谢私信或在评论区留言
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424165249825.jpeg#pic_center)