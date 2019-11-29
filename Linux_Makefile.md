### Makefile



##### make 解释器

makefile/Makefile

linux:  makefile.gcc    makefile.inc

Win:  makefile.mvc



##### 编写规则：

目标对象:依赖对象

tab 编译命令



##### 解释规则：

1）对比时间：判断目标对象和依赖对象的最后一次修改时间，若目标对象最后一次修改时间比依赖对象的最后一次修改时间距离现在近，则不用生成目标对象。反之，则生成目标对象



2）判断依赖对象是否存在，如果存在，直接执行编译命令，如果不能存在，则在makefile之后的编写中去查找生成依赖对象的方法，如果找到，则判断生成依赖对象的方法是否存在，如果存在，则生成依赖对象，继续生成目标对象。如果不存在，则报错

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_7268\Image.png)

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_31947\Image.png)

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_10365\Image.png)

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_29186\Image.png)

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_2999\Image.png)

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_3153\Image.png)

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_30831\Image.png)

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_27026\Image.png)



3）make 永远只为了生成第一个目标对象而去服务

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_23743\Image.png)

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_23779\Image.png)



###### 伪目标

PHONY:[伪目标]    

作用：定义一个伪目标，屏蔽比较时间，一直都生成目标对象



###### makefile 中的预定义变量

​	$@    目标对象

​	$^     所有依赖的对象

​	$<     依赖的第一个对象

![Image](C:\Users\28195\AppData\Local\Temp\chrome_drag7508_15302\Image.png)



###### makefile 中的自定义变量

```
SRC=$(wildcard *.c)                     # 把当前文件夹下所有的.c文件都写在变量 SRC 中，以空格分隔
OBJ=$(patsubst %.c, %.o, $(SRC))        # 将所有的.c文件名称中的.c以.o的名称形式写在对象 OBJ 中，以空格分隔，并没有生成任何.o文件
BIN=out                                 #

$(BIN):$(OBJ)                           # 目标对象:依赖对象
    gcc $^ -o $@                        # gcc test.c -o out

%.o:%.c                                 # 目标对象:依赖对象
    gcc -c $< -o $@                     # gcc -c test.c -o test.o

.PHONY:clean                            # 伪目标
clean:                                  # make clean
    rm $(BIN) $(OBJ)                    # rm out test.o
```

