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



------



Makefile

~~~ctrl+C复制代码
# flags
CC = gcc
CFLAGS = -Wall
LFLAGS = 

# args
RELEASE =0
BITS =

# [args] 生成模式. 0代表debug模式, 1代表release模式. make RELEASE=1.
ifeq ($(RELEASE),0)
    # debug
    CFLAGS += -g
else
    # release
    CFLAGS += -static -O3 -DNDEBUG
    LFLAGS += -static
endif

# [args] 程序位数. 32代表32位程序, 64代表64位程序, 其他默认. make BITS=32.
ifeq ($(BITS),32)
    CFLAGS += -m32
    LFLAGS += -m32
else
    ifeq ($(BITS),64)
        CFLAGS += -m64
        LFLAGS += -m64
    else
    endif
endif


.PHONY : all clean

# files
TARGETS = gcc64_make
OBJS = gcc64_make.o

all : $(TARGETS)

gcc64_make : $(OBJS)
    $(CC) $(LFLAGS) -o $@ $^


gcc64_make.o : gcc64_make.c
    $(CC) $(CFLAGS) -c $<


clean :
    rm -f $(OBJS) $(TARGETS) $(addsuffix .exe,$(TARGETS))
~~~



为了控制条件编译，定义了RELEASE、BITS这两个变量，分别赋初值。然后用ifeq判断RELEASE、BITS变量的值，分别加上不同的参数。
　　因赋有初值，直接执行“make”时，编译得到的是默认位数的debug版。
　　若在执行make时给变量赋值，将会得到不同的版本——
make RELEASE=0：（默认位数的）debug版。
make RELEASE=1：（默认位数的）release版。
make BITS=32：32位（的debug）版。
make BITS=64：64位（的debug）版。
make RELEASE=0 BITS=32：32位的debug版。
make RELEASE=0 BITS=64：64位的debug版。
make RELEASE=1 BITS=32：32位的release版。
make RELEASE=1 BITS=64：64位的release版。


　　该makefile的代码风格是精心设计的，可以很方便的扩展——
需要增加代码文件或依赖关系时，修改“# files”之后的内容。
需要调整编译参数时，修改前半部分的参数变量。
需要增加新的条件编译参数时，在“# args”定义一个变量并赋初值，然后再在后面用“ifeq”判断变量来调整编译参数。

　　最后的“rm -f $(OBJS) $(TARGETS) $(addsuffix .exe,$(TARGETS))”是为了兼容MinGW、TDM-GCC等Windows下的GCC编译器而设计的——
装好MSYS，再配置一下PATH环境变量，Windows中也可以使用rm命令删除文件。
因Windows下的可执行文件的扩展名是exe，所以使用了addsuffix函数增加“.exe”扩展名。
因Linux下不会生成.exe可执行文件，而Windows下不会生成无扩展名的可执行文件，导致rm会因找不到文件而报错。这时可以加上-f参数忽略该错误。

