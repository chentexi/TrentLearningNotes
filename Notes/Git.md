Git

Git、GitHub、Gitee、GitLab大纲介绍

# 1.Git

## 集中式版本控制

### SVN

![image-20210523181918450](Image/image-20210523181918450.png)

## 分布式版本控制

### Git

​		![image-20210523181933490](Image/image-20210523181933490.png)

## Git工作机制

​	git add .  --> (暂存区可以随时删除)--> git commit -m "" -->(会生成历史本版不能删除) ->git push-->(推送到远程库)

![image-20210523181958552](Image/image-20210523181958552.png)

## Git和代码托管中心

![image-20210523172242976](Image/image-20210523172242976.png)

1. GitLab:组建自己的代码托管中心(局域网下)
2. GitHub:外网代码托管中心
3. Gitee: 国内的

## Git安装

![image-20210523175837637](Image/image-20210523175837637.png)

选择 Git安装位置，要求是非中文并且没有空格的目录，然后下一步。

![image-20210523175942262](Image/image-20210523175942262.png)

Git选项配置，推荐默认设置，然后下一步。

![image-20210523180009471](Image/image-20210523180009471.png)

Git安装目录名，不用修改，直接点击下一步。

![image-20210523180036180](Image/image-20210523180036180.png)

Git的默认编辑器，建议使用默认的 Vim编辑器，然后点击下一步。

![image-20210523180104925](Image/image-20210523180104925.png)

默认分支名设置，选择让 Git决定，分支名默认为 master，下一步。

![image-20210523180148384](Image/image-20210523180148384.png)

修改 Git的环境变量，选第一个，不修改环境变量，只在 Git Bash里使用 Git。

![image-20210523180218129](Image/image-20210523180218129.png)

选择后台客户端连接协议，选默认值 OpenSSL，然后下一步。

![image-20210523180243144](Image/image-20210523180243144.png)

配置Git文件的行末换行符， Windows使用 CRLF Linux使用 LF，选择第一个自动
转换，然后继续下一步。

![image-20210523180315175](Image/image-20210523180315175.png)

选择Git终端类型，选择默认的 Git Bash终端，然后继续下一步。

![image-20210523180339251](Image/image-20210523180339251.png)

选择Git pull合并的模式，选择默认，然后下一步。

![image-20210523180406681](Image/image-20210523180406681.png)

选择Git的凭据管理器，选择默认 的跨平台的凭据管理器 ，然后下一步 。

![image-20210523180427103](Image/image-20210523180427103.png)

其他配置，选择默认设置，然后下一步。

![image-20210523180449085](Image/image-20210523180449085.png)

实验室功能，技术还不成熟， 有已知的 bug，不要勾选，然后点击右下角的 Install按钮，开始安装 Git。

![image-20210523180519120](Image/image-20210523180519120.png)

# 2.1 Git常用命令

| 命令                                    |      作用      |
| :-------------------------------------- | :------------: |
| git config global user.name 用户名 设置 |  设置用户签名  |
| git config global user.email 邮箱       |  设置用户签名  |
| git init                                |  初始化本地库  |
| git status                              | 查看本地库状态 |
| git add 文件名                          |                |
| git commit -m "日志信息" 文件名         |  提交到本地库  |
| git reflog                              |  查看历史记录  |
| git reset --hard 版本号                 |    版本穿梭    |
| git log                                 | 详细的历史记录 |

## Git 操作命令

### 修改git的名字

#### 修改全局的名字与邮箱

git config --global user.name 'xxx'

git config --global user.email 'xxx'

#### 修改本身项目的名字

git config --local user.name 'xxx'

git config --local user.email 'xxx'

![image-20210721101719774](Image/image-20210721101719774.png)

config有几个常用参数， 缺省等于local

* local 只对当前仓库有效
* global 对登录⽤户所有仓库有效    
* system 对系统的所有⽤户有效
* unset 清除某个信息
  	git config --unset --local user.name

显示config的配置可以 加 --list
	       git config --list --local

#### git有代理时的操作代理

Clone failed unabletoaccess xxx  :failed to connect to xxx port  :==Bad access==当出现这种情况时则进行以下操作,更换正确的的代理地址

注销代理

```git
git config --global/local --unset http.proxy 
```

设置代理

```
git config --global/local  http.proxy http://xxx(127.0.0.1:8080)
```

如果还是Bad access错误时则更换正确的仓库地址

![image-20210729182108237](Image/image-20210729182108237.png)

```git
git remote set-url origin http://xxx仓库项目地址xxxxxx
```

最终查看全局或者本地的配置

```git
//本地(本项目)
git config --list --local
//全局
git config --list
```





### 编辑

 1. 编辑vim  (vim 文件名)

    vim Notes/Git.md 

 2. 编辑退出

    按 ESC，左下角就可以进行输入

    `:w` 保存但不退出

    `:wq` 保存并退出

    `:q` 退出

    `:q!` 强制退出，不保存

    `:e!` 放弃所有修改，从上次保存文件开始再编辑命令历史 

### git 常用命令

 1. git status 查看本地库状态

    ![image-20210523205115040](Image/image-20210523205115040.png)

2. git add 

   git add Notes/Git.md

   ![image-20210523205353674](Image/image-20210523205353674.png)

   git add .

   ![image-20210523205439707](Image/image-20210523205439707.png)

3. git commit -m '日志记录'

   git commit -m '添加笔记文件git'

   ![image-20210523205603451](Image/image-20210523205603451.png)

4. git push (远程推送)

   ![image-20210523205749275](Image/image-20210523205749275.png)

5. git reflog

   ![image-20210523210100230](Image/image-20210523210100230.png)

6. git log

    ![image-20210523210155668](Image/image-20210523210155668.png)

7. cat 文件名

   cat Notes/Git.md

   ![image-20210523213827678](Image/image-20210523213827678.png)

### 回溯版本 

Git reset --hard 版本号 (穿梭到指定的版本)

git reset head

这样的话，我们就可以回到开始了。但这个操作好像仅限于在master之类的分支上，如果是在一个新的远程分支，就会出现：

Unstaged changes after reset

解决的办法如下2种办法：

1. git add .

git reset --hard

2. git stash

git stash drop

出现这种现象的原因好像是因为在新分支上，repos感知不到这个阶段的改变，你要用 add 或 stash, 让其知晓，才能做想要的回滚。

### Git 分支

![image-20210523214422641](Image/image-20210523214422641.png)

### 什么是分支

​	在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是一个单独的副本。（分支底层其实也是指针的引用）

![image-20210523214635361](Image/image-20210523214635361.png)

### 分支操作

| 命令                         |             作用             |
| ---------------------------- | :--------------------------: |
| git branch 分支名            |           创建分支           |
| git branch -v                |           查看分支           |
| git branch -d 分支名\|版本号 |           删除分支           |
| git checkout 分支名          |           切换分支           |
| git merge 分支名             | 把指定的分支合并到当前分支上 |

1. 创建分支

   git branch v

   ![image-20210523215143681](Image/image-20210523215143681.png)

2. 查看分支

   git branch

   ![image-20210523215356560](Image/image-20210523215356560.png)

   git branch -v

   ![image-20210523215617251](Image/image-20210523215617251.png)

3. 删除分支

   git branch -d v

   ![image-20210523220107905](Image/image-20210523220107905.png)

4. 修改分支

![image-20210523221655216](Image/image-20210523221655216.png)
   =======
在hox-fix分支下面进行修改不影响master分支

 

5. 切换分支

   git checkout 分支名

   ![image-20210523220656647](Image/image-20210523220656647.png)



6. 合并分支

   git merge 分支名
   
   ![image-20210523222614248](Image/image-20210523222614248.png)

冲突产生的原因:
   合并分支时，两个分支在==同一个文件的同一个位置==有两套完全不同的修改。 Git 无法替我们决定使用哪一个。必须 ==人为决定== 新代码内容。

查看状态（检测到有文件 有两处修改）
![image-20210523222843945](Image/image-20210523222843945.png)

解决冲突:

 1. 编辑有冲突的文件，删除特殊符号，决定要使用的内容

    特殊符号：
    <<<<<<< HEAD 当前分支的代码 ======= 合并过来的代码 >>>>>>> hot-fix

2. 添加到暂存区

   git add Notes/Git.md

3. 执行提交（注意：此时使用git commit 命令时不能带文件名）

   git commit -m "merge hot-fix"



# 2.Git团队协作机制

## 2.1 团队协作

![image-20210523233537693](Image/image-20210523233537693.png)

## 2.2 跨团队协作

![image-20210523233616593](Image/image-20210523233616593.png)

# 3.Git-->GitHub

## 创建远程仓库

1. ![image-20210523234107139](Image/image-20210523234107139.png)
2. 
3. ![image-20210523234135071](Image/image-20210523234135071.png)

## 远程仓库操作

| 命令                               | 作用                                                      |
| ---------------------------------- | --------------------------------------------------------- |
| git remote -v                      | 查看当前所有远程地址别名                                  |
| git remote add 别名 远程地址       | 起别名                                                    |
| git push 别名 分支                 | 推送本地分支上的内容到远程仓库                            |
| git clone 远程地址                 | 将远程仓库的内容克隆到本地                                |
| git pull 远程库地址别名 远程分支名 | 将远程仓库对于 分支最新内容拉下来后与当前本地分支直接合并 |

1. git remote -v 

   ![image-20210523234706031](Image/image-20210523234706031.png)

2. git remote add 别名 远程地址

   ![image-20210523235132366](Image/image-20210523235132366.png)

3. git push 别名 分支

   ![image-20210523235325202](Image/image-20210523235325202.png)

4. git clone 远程地址

5. git pull 远程库地址别名 远程分支名

   ![image-20210524000204461](Image/image-20210524000204461.png)

## 3.3 邀请团队

1. 
2. ![image-20210524002343641](Image/image-20210524002343641.png)

2. 填入团队成员

![image-20210524002500559](Image/image-20210524002500559.png)

3. 复制地址并通过微信钉钉等方式发送给该用户，复制内容如下：

![image-20210524002535564](Image/image-20210524002535564.png)

4. 在 atguigulinghuchong这个账号 中的 地址 栏 复制 收到邀请 的 链接 ，点击接受邀请。

![image-20210524002608549](Image/image-20210524002608549.png)

5. 成功之后团队成员可以在这个账号上看到 git-Test的远程仓库。

   ![image-20210524002758829](Image/image-20210524002758829.png)

6. 令狐冲可以修改内容并 push到远程仓库。

   ![image-20210524002832995](Image/image-20210524002832995.png)

![image-20210524002859163](Image/image-20210524002859163.png)

7. 回到 主账号的 GitHub远程仓库中可以看到，最后一次是 lhc提交的。

![image-20210524002929577](Image/image-20210524002929577.png)

![image-20210524002944560](Image/image-20210524002944560.png)

## 3.4 跨团队协作

1. 将远程仓库的地址复制发给邀请跨团队协作的人，比如东方不败。

   ![image-20210524003231309](Image/image-20210524003231309.png)

2. 在东方不败的 GitHub账号里的地址栏复制收到的链接，然后点击 Fork将项目叉到自己的本地仓库

   ![image-20210524003429499](Image/image-20210524003429499.png)

![image-20210524003550062](Image/image-20210524003550062.png)

3. 东方不败就可以在线编辑叉取过来的文件。

   ![image-20210524003704322](Image/image-20210524003704322.png)

4. 编辑完毕后，填写描述信息并点击左下角绿色按钮提交。

   ![image-20210524003731953](Image/image-20210524003731953.png)

5. 接下来点击上方的 Pull请求，并创建一个新的请求

   ![image-20210524003758309](Image/image-20210524003758309.png)

![image-20210524003819753](Image/image-20210524003819753.png)

6.回到岳岳 GitHub账号可以看到有一个 Pull request请求 。

![image-20210524003847692](Image/image-20210524003847692.png)

![image-20210524004032639](Image/image-20210524004032639.png)

7. 如果代码没有问题，可以点击 Merge pull reque合并代码。

![image-20210524004057464](Image/image-20210524004057464.png)



## 3.5 SSH免密登录

我们可以看到远程仓库中还有一个SSH的地址，因此我们也可以使用 SSH进行访问 。

![image-20210524004306795](Image/image-20210524004306795.png)

具体操作:

1.

```git
进入当前用户的家目录
Layne@LAPTOP Layne MINGW64 /d Git Space/SH0720 (
$ cd
删除 .ssh 目录
Layne@LAPTOP Layne MINGW64
$ rm rvf .ssh
removed '.ssh/known_hosts'
removed directory '.ssh'
运行命令生成 ssh 秘钥目录 注意：这里 C 这个参数是大写的 C
Layne@LAPTOP Layne MINGW64
$ ssh keygen t rsa C atguiguyueyue@aliyun.com
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/ Layne /.ssh/id_
Created directory '/c/Users/ Layne /.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/ Layne /.ssh/id_
Your public key has been saved in /c/Users/ Layne /.ssh/id_rsa.
The key fingerprin t is:
SHA256:7CPfRLITKcYDhaqpEDeok7Atvwh2reRmpxxOC6dkY44
atguiguyueyue@aliyun.com
The key's randomart image is:
++------[RSA
| ..
| ..
| . ..
|+ + o . .
|oO . = S .
|X . .. + =
|+@ * .. = .
|X.&o+. o =
|Eo+Oo . .
++--------[
进入 .ssh 目录查看文件列表
Layne@LAPTOP Layne MINGW64
$ cd .ssh
Layne@LAPTOP Layne MINGW64 ~/.ssh
$ ll a
total 21
drwxr xr x 1 Layne 197609 0 11 月 25 19:27
drwxr xr x 1 Layne 197609 0 11 月 25 19:27
rw r r 1 Layne 197609 1679 11 月 25 19:27 id_rsa
rw r r 1 Layne 197609 406 11 月 25 19:27 id_rsa.pub
查看 id_rsa.pub 文件内容
Layne@LAPTOP Layne MINGW64 ~/.ssh
$ cat id_rsa.pub
ssh rsa
AAAAB3NzaC1yc2EAAAADAQABAAABAQDRXRsk9Ohtg1AXLltsuNRAGBsx3ypE1O1Rkdzpm
l1woa6y6G62lZri3XtCH0F7GQvnMvQtPISJFXXWo+jFHZmqYQa/6kOIMv2sszcoj2Qtwl
lGXTPn/4T2h/cHjSHfc+ks8OYP7OWOOefpOCbYY/7DWYrl89k7nQlfd+A1FV/vQmcsa1L
P5ihqjpjms2CoUUen8kZHbjwHBAHQHWRE+Vc371MG/dwINvCi 8n7ibI86o2k0dW0+8SL+
svPV/Y0G9m+RAqgec8b9U6DcSSAMH5uq4UWfnAcUNagb/aJQLytrH0pLa8nMv3XdSGNNo
AGBFeW2+K81XrmkP27FrLI6lDef atguiguyueyue@aliyun.com
```

2. 复制id_rsa.pub文件内容，登录GitHub，

   点击用户头像-->Settings--> SSH and GPG keys-->New SSH Key-->

![image-20210524004909166](Image/image-20210524004909166.png)

# 4.Git-->IDEA

 ## 4.1 忽略文件

1. 创建忽略规则文件xxx.ignore

这个文件的存放位置原则上在哪里都可以，为了便于让~/.gitconfig文件引用，建议也放在用户家目录下
git.ignore文件模版内容如下：

```ignore
# Compiled class file
*.class
# Log file
*.log
# BlueJ files
*.ctxt
# Mobile Tools for Java (
.mtj.
# Package Files
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar
# virtual machine crash logs, see
http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
.classpath
.project
.settings
target
.idea
*.iml
```

2. 在 .gitconfig文件中引用忽略配置文件（此文件在 Windows的家目录中）

```
[user]
		name = Layne
	email = Layne @atguigu.com
[core]
excludesfile = C:/Users/ asus git .ignore
注意：这里要使用“正斜线（ （//）”，不要使用“反斜线
```

## 4.2 切换版本

![image-20210524232220447](Image/image-20210524232220447.png)

## 4.3 创建分支

![image-20210524232435502](Image/image-20210524232435502.png)

## 4.4 切换分支

![image-20210524232743473](Image/image-20210524232743473.png)

## 4.5 合并分支

![image-20210524232845953](Image/image-20210524232845953.png)

## 4.6 解决冲突

如图所示，如果master分支和 hot-fix分支都修改了代码，在合并分支的时候就会发生冲突。

![image-20210524233952886](Image/image-20210524233952886.png)

![image-20210524234011499](Image/image-20210524234011499.png)

我们现在站在master分支上合并 hot-fix分支，就会发生代码冲突。

![image-20210524234306249](Image/image-20210524234306249.png)

点击 Conflicts框里的 Merge按钮，进行手动合并代码。

![image-20210524234521877](Image/image-20210524234521877.png)

手动合并完代码以后，点击右下角的 Apply按钮。

![image-20210524234546606](Image/image-20210524234546606.png)

代码冲突解决，自动提交本地库。

![image-20210524234613181](Image/image-20210524234613181.png)

# 5. Git-->Gitee

# 6. Git-->GitLab



