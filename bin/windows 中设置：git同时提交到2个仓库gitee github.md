windows 中设置：git同时提交到2个仓库gitee github

1.进入项目根目录

![1585740957650](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585740957650.png)

点击查看，显示隐藏文件夹

![1585740977359](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585740977359.png)

出现git文件夹

![1585740991430](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585740991430.png)	

3.进入.git文件夹

![1585741018334](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585741018334.png)

4.编辑config

![1585741049358](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585741049358.png)

代码如下

```html
[core]
   repositoryformatversion = 0
   filemode = false
   bare = false
   logallrefupdates = true
   symlinks = false
   ignorecase = true
[submodule]
   active = .
[remote "origin"]
   url = https://gitee.com/stylesmile/snow.git
   fetch = +refs/heads/*:refs/remotes/origin/*
   url = https://github.com/stylesmile/snow.git
   
[branch "master"]
   remote = origin
   merge = refs/heads/master
   
[remote "github"]
    url = https://github.com/stylesmile/snow.git
    fetch = +refs/heads/*:refs/remotes/gitee/*
    tagopt = --no-tags
```

再次push ,就会同时提交到2个仓库

https://msd.misuland.com/pd/2884250137616452150 tagopt = --no-tags 使用一句 git 命令将仓库的改动推送到所有的远端