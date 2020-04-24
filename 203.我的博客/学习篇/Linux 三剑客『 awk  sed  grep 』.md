## 前言 
本篇博客意在讲解 Linux 常用的命令 ---  linux 三剑客『 awk / sed / grep 』，老大 awk 最擅长取列，老二 sed 最擅长取行，老三 grep 最擅长过滤，是 Linux 运维人员必会的三个基本命令 ~~~

-----

@[toc]

-----

### awk ---- 数据分割、处理

#### 使用说明

awk 以字段为单位进行处理(其实就是把一行的数据分割，然后进行处理)
**格式**：`awk  [options]  '{pattern + action}'  {filenames}`
 - `pattern` ：模式，即条件，可理解为'找谁'
 - `action` ：动作，可理解为'干啥'


> $0  当前行整行，代表一整行的数据
> $1  当前行被分割符划分成N段，$1表示第一个字段
> $2  代表第二个字段，依次类推 $3...
> $NF  当前行被分割后的最后一列（字段）
> NF  每一行拥有的字段总数
> NR   目前处理的是第几行的数据
> FS   目前的分隔字符
> **命令格式** ：awk   '条件{命令1} 条件{命令2}...'   file_name

#### 使用演示
~~~
awk -F  ":" 'NR>=2 && NR<=6{print NR,$1}'  /etc/passwd2 
~~~
- `-F` 指定分隔符为冒号，相当于以“：”为菜刀，进行字段的切割。
- `NR>=2 && NR<=6`：这部分表示模式，是一个条件，表示取第2行到第6行。
- `{print NR,$1}`：这部分表示动作，表示要输出NR行号和$1第一列。
- /etc/passwd2 为文件名 (路径)
~~~
awk 'NR<6{print $1 "\t" $2}' file_name  
~~~
`awk   'NR<6{print   $1  "\t"  $2 }'     file_name` 把 file_name 文件中的前五行的第一列，第二列的数据列出来 (以 [ tab ] 或空格键分隔)

~~~
awk 'BEGIN {count=0;print "[start] user count is ",count} {count=count+1;print $0} END{print "[end] user count is ",count}' file_name
~~~
 - `BEGIN { }` 指定了处理文本之前需要执行的操作
 - `END { }` 指定了处理所有行之后需要执行的操作

~~~
awk 'NR>22 && NR<31' /etc/services
~~~
`awk 'NR>22 && NR<31' /etc/services` 取出文件/etc/services的23～30行

~~~
awk -F "[ /]+" '$1~/^(ssh)$|^(http)$|^(https)$|^(mysql)$|^(ftp)$/{print $1,$2}' /etc/services |sort|uniq
~~~
取出常用服务端口号

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424173710375.png#pic_center)


\
**小结**
- /etc/passwd 文件中第一列是帐户，第三列是用户ID，该文件以 `:` 号分隔，一行代表一个用户，记录关于用户的各种信息。 
- print 默认带有换行符，printf 没有 
- 像 \n,\t, 这种符号应该用双引号括起来 
- NR，NF等变量要用大写，并且不需要 $ 



-----


### sed ---- 对数据进行操作（增、删、替、选）
#### 使用说明
sed 实现数据的增加，删除，替换，选取等(按顺序逐行读取文件，不会影响源文件)

**格式** ：`sed [option] [command] [file]`
 - `option` :
    a   ∶新增
    c   ∶取代
    d   ∶删除
    i   ∶插入
    p  ∶打印
    s  ∶取代

#### 使用演示
~~~
sed ’1d' ghostwu.com
~~~
`d` 代表删除, `d 前面的数字`代表删除第一行，该命令不会修改文件本身

~~~
sed '$d' ghostwu.txt      # 删除最后一行，$代表最后一行
sed '1,2d' ghostwu.txt    # 删除第一行到第二行
sed '2,$d' ghostwu.txt    # 删除第二行到最后一行
~~~

~~~
sed -n '/you/p' ghostwu.txt
~~~
查找包含 `you` 的行，`/you/` 这是正则表达式， `p`是打印，要跟 `n` 结合起来用

~~~
sed '1,2a 你好啊' ghostwu.txt   				# 在第一行和第二行的后面，增加一行“你好啊"
sed '1a 你好啊\n很高兴认识你' ghostwu.txt    	# 在第一行的后面，增加2行数据，\n 换行
sed '1c 你好啊' ghostwu.txt      			# 将第一行数据替换为“你好啊"
sed -i '$a 你好啊' ghostwu.txt   			# 在最后一行新增“你好啊"，该命令会修改文件本身
~~~

------


### grep ---- 文本搜索
#### 使用说明

grep 是：强大的文本’搜索’工具
**格式**：`grep   -n 'word'  file_name`
在 `file_name`文件中找到 `word` 所在的所有行并显示，`-n` 表示显示行号

```
grep -n 'word' file_name    # 在file_name文件中找到word所在的所有行并显示。-n 为显示行号
grep 'w[ea]ll' file_name    # 在file_name文件中找到wall 或者是well 所在的所有行并显示
grep 'w[^e]ll' file_name    # 在file_name文件中找到”非well” 所在的所有行并显示
grep '^w' file_name       # 在file_name文件中找到以w开头的所有行并显示
grep 'g..le' file_name      # 在file_name文件中找到如goole/gkkle..的所有行并显示
grep 'g*g' file_name        # 在file_name文件中找到g/gg/ggg等的所有行并显示（*代表重复前一个字符无穷次）
```

#### 使用演示
先准备一个文件 `file_name`，并随便写入一些数据，例：
~~~
goole
ggggsgsgah
shshshglgaa
ggsvr
geele
well
wall
waall
word
world
~~~

查看文件 file_name 中的内容`cat file_name`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424180614743.png)
下图是演示结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424181205214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjE5NDE2MQ==,size_16,color_FFFFFF,t_70)

------

◆ 回到开头 @[目录](https://blog.csdn.net/weixin_42194161/article/details/105733509#_0)
 - [awk](https://blog.csdn.net/weixin_42194161/article/details/105733509#awk___12)
 - [sed](https://blog.csdn.net/weixin_42194161/article/details/105733509#sed___75)
 - [grep](https://blog.csdn.net/weixin_42194161/article/details/105733509#grep___115)

◆ 其他博客 @ [https://blog.csdn.net/姜小逗](https://blog.csdn.net/weixin_42194161)


◆ 相关博客

 - [学习篇 | Linux环境中 -- 静态库&动态库的生成与使用](https://blog.csdn.net/weixin_42194161/article/details/104593893)
 - [学习篇 | LINUX 内核的文件系统 -- ext2](https://blog.csdn.net/weixin_42194161/article/details/104588023)



----

 感谢阅读本篇博客，如果有不错的发现和建议，感谢私信或在评论区留言
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424165249825.jpeg#pic_center)