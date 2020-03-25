------



做以下操作从您的终端：

```
git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime
sh ~/.vim_runtime/install_awesome_vimrc.sh
```



或者：来安装所有带有主目录的用户

`git clone --depth=1 https://github.com/amix/vimrc.git /opt/vim_runtime`

`sh ~/.vim_runtime/install_awesome_parameterized.sh /opt/vim_runtime --all`

当然，`/opt/vim_runtime` 可以是任何目录，只要指定的所有用户都具有读访问权

​	

##### VIM 的两种配置版本

##### （一）基本版：

基本版本只有一个文件，没有插件。复制的基本。然后把它粘贴到你的 `vimro` 上，基本版本对于安装在远程服务器上非常有用，因为在远程服务器上不需要很多插件，也不需要进行很多编辑

```
git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime
sh ~/.vim_runtime/install_basic_vimrc.sh
```



如何在 Linux 上安装

​	如果您将 vim 别名改为 vi 而不是 vim，请确保别名为：`alias vi=vim`. 否则, `apt-get install vim`



如何更新到最新版本？

​	只需做一次 git rebase !

```
cd ~/.vim_runtime
git pull --rebase
python update_plugins.py
```



-------------



使用截图：

- 编辑 Python 文件时的颜色：
- 用 mru.vim 插件打开最近打开的文件：
- 终端窗口的 NERD Tree 插件：



- 分心自由模式使用 goyo.vim 和 vim2-zenroom2：



------



包含插件：（注：建议阅读下面的插件文档以更好地理解他们）

- 







---------

