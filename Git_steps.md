git 的使用：

- git status ./
- git add .    ---->    然后 git status ./
- git commit -m "add readme"    引号内的内容代表你的git日志，可更改
- git push -u origin master



[https://www.centos.bz/2018/03/centos7%E6%90%AD%E5%BB%BAgit%E5%8F%8A%E5%AE%89%E8%A3%85%E4%BD%BF%E7%94%A8/](https://www.centos.bz/2018/03/centos7搭建git及安装使用/)   Centos7搭建Git及安装使用



---



**小白版**：

1. 初始化一个项目

git config --global user.name "你的名字或昵称"
git config --global user.email "你的邮箱"

2. 初始使用：

git init 

git remote add origin <你的项目地址> // 注:项目地址形式为:https://gitee.com/xxx/xxx.git或者 git@gitee.com:xxx/xxx.git

如果想长期记住密码，如下操作：

git remote add origin http://您的账号:你的密码@gitee.com/您的项目地址.git

3. 克隆一个项目

git clone <项目地址>

4. 完成第一次提交

git pull origin master
<这里需要修改/添加文件，否则与原文件相比就没有变动>
git add .
git commit -m "第一次提交"
git push origin master

##################### 至此完成远程项目的本地初始化 #######################

* 如果你原本使用的仓库地址，可以执行以下命令

  ```csharp
  // 删除原本的ssh仓库地址
  git remote rm origin // origin 代表你原本ssh地址的仓库的别名
  // 新增http地址的仓库
  git remote add origin https://gitee.com/username/project.git
  ```

- 按照以下设置记住密码十五分钟：

  git config --global credential.helper cache

- 如果你想自定义记住的时间，可以这样：	

  ```csharp
  git config credential.helper 'cache --timeout=3600' //这里记住的是一个小时，如需其他时间，请修改3600为你想修改的时间，单位是秒
  ```

- 你也可以设置长期记住密码：

  ```csharp
  git config --global credential.helper store  // 建议还是用上面第2点提到的记住密码方式，方便多项目操作
  ```

- Git提交代码到码云

  ```csharp
  git add .    // 提交当前目录下的所有文件；
  git commit -m '注释'    // 添加注释
  git pull     //下载服务器代码 这是为了同步最新远程文件到本地，防止冲突
  git push    // 传代码至服务器
  ```







gitee 贡献度问题：

gitee账号与本地 config 不同：(多了引号、输入错误，email 账号不对等等)

git config user.name

git config user.email



git remote 问题：

git remote rm origin 

git remote -v

git remote add origin http://gitee.com/.../....git







`重新`生成 ssh：

ssh-keygen -t rsa -C “xxxxx@xxxxx.com”

验证是否连接成功：ssh -T git@gitee.com