**把 gitee 仓库迁移回 github 中**



注意：这些方法都可以将提交记录 commit 和文件一并导入



方法一：在GitHub官网创建仓库的时候直接导入

进入官网，登录（注册）账号

![1585748031696](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585748031696.png)

先在github点顶部栏的`New repository`来创建一个私有仓库（虽然有Import repository选项但建议别直接使用这个，后面你就知道了），仓库名可以跟gitee一样。

![1585748194309](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585748194309.png)

进入新创建的仓库页面，在最底下有个`Import code`，点它进入导入页面（这个界面跟创建仓库时直接点Import repository的界面其实差不多）。

![1585748267883](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585748267883.png)

![1585748282419](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585748282419.png)

敲黑板！这块是全教程最重要一点！ 填写gitee的仓库地址，例如https://gitee.com/liyifeng1994/study-xxx.git，后面的.git带不带都行。但是！这种仓库地址只适合gitee公共仓库，如果你的仓库是私有的（不是私有我干嘛放gitee？），那么你就要在仓库地址上加上gitee的用户名和密码，例如https://liyifeng1994:password@gitee.com/liyifeng1994/study-xxx.git，注意地址前面的:和@。用户名一般是你仓库地址上面的用户名，并非你在gitee的某个登录邮箱帐号，邮箱包含了@符号反而造成地址解析错误。怎么检验你输入的地址是否正确？可以在本地电脑通过命令git clone https://liyifeng...看下能否把仓库下下来。确认仓库地址无误后点击Begin import完成导入。

【图三】![1585740828663](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585740828663.png)

什么！500？是的！你没看错，页面一般情况下都会返回500错误页面（反正我是几乎没遇到正常的页面）。莫方，等个几秒后点一下浏览器返回，然后刷新导入页面，就会看到页面显示导入的进度条和一些提示文案。大概再过了2-3分钟（视仓库文件大小而定），刷新仓库主页就能看到所有代码文件和commits都回来了（开心~）。【图四】

![1585740819484](C:\Users\28195\AppData\Roaming\Typora\typora-user-images\1585740819484.png)



方法二：克隆到本地，再使用命令导入到 GitHub 中

1. 先在github创建一个私有仓库，跟上面步骤1一样。
2. 把gitee上的仓库下载到本地（如果本地已有请跳过）。
   git clone https://gitee.com/liyifeng1994/study-xxx.git
3. 移除gitee远端（不移除也可以，但github要改名不能也叫origin）
   git remote remove origin
4. 添加github远端
   git remote add origin https://github.com/liyifeng1994/study-xxx.git
5. 推送到github的master（如果有多个分支需要一个一个推）
   git push -u origin master



-----

如果仓库在电脑本地已经存在且没有过多分支，可以直接使用第二种。当然，我更愿意使用第一种来的方便。其实github的导入并非仅限于gitee平台，你也可以把gitlab、bitbucket平台的仓库迁移到github，本教程只是用国内知名的中文代码托管平台gitee(码云)来举个例子。