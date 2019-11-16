# git介绍

git是一个版本控制软件。

# git三个工作区域

| 工作目录       | 暂存区                       | git仓库                    |
|----------------|------------------------------|----------------------------|
| 本地的工作目录 | 位于.git仓库中的一个特殊区域 | .git文件夹，版本控制的核心 |

git版本控制的核心就是对该三个区域的操作

# git操作

## 配置

- git config --global 配置当前用户git设置
- git配置文件位于~/.gitconfig文件内
- 通常配置 user.name user.email来标识自己

## 初始化仓库

- git init 初始化当前目录为git仓库
- git clone URL 克隆远程仓库

## 基本操作

- 工作区=>暂存区 git add file
- 暂存区=>版本库 git commit -m "commit message"
- 版本库=>工作区 git restore file （注意这是回退操作，如果在工作区的更改未提交这个命令会用版本库中的文件覆盖工作区中的文件，未提交的更爱无法在还原了）
- 暂存区=>工作区 git restore --stage file （放弃add操作，工作区中的更改并未被删除）

## 分析不同

- 工作区～暂存区：git diff
- 工作区～版本库：git diff HEAD
- 暂存区～版本库：git diff --cached

## 删除操作

- 工作区：rm file
- git rm file 将删除动作推送到暂存区
- git commit -m "delete file" 在版本库中执行删除操作

## 版本回退

- git reset --hard HEAD^/commitid
- HEAD^表示上一个提交版本，HEAD^^表示上上个，以此类推。commitid是每次推送的id号
- git log 可以查看版本库历史情况
- git reflog 可以查看所有分支的所有提交情况

## 分支管理

- git branch 查看当前分支
- git branch name 创建新分支
- git switch name 切换当前分支
- git marge name 合并分支到当前分支
- git branch -d name 删除分支

## HEAD指针
git版本库中维护了一个HEAD指针，在版本回退中之所以git可以快速操作，实际上就是他直接将HEAD指针指向了某一次提交，这样就可以将该次提交作为当前的工作环境。  
在分支管理中相同。默认情况下git仅仅一个分支master，而HEAD就是指向该分支的某一次提交。而切换分支就是将HEAD指针直接指向该分支地一次提交即可。

## 远程仓库
使用github作为远程仓库使用的方法：
- ssh-keygen -t rsa -C "注册github的邮箱" 会在~/.ssh/下生成一个id_rsa.pub文件
- 将id_rsa.pub文件中的内容粘贴到github下的SSH keys中即可
- 在github下new repository 新建一个版本库，其中会得到一个该仓库的url通常为：git@github.com:hncjygd/study.git这个样子。
- 管理远程仓库
    - git remote add origin git@github.com:hncjygd/study.git 创建一个远程仓库，并且指定了一个名字为origin(这个名字可以随意取，但是在克隆操作某个项目后，至少可以看到一个名为origin的远程仓库，git默认使用这个名字，所以约定俗成第一个远程仓库都叫这个名字。)每个版本库可以有多个远程仓库，例如我们创建了一个名字为origin指向github的远程仓库
    - git remote -v 查看当前配置下有那些远程仓库。
    - git fetch remote-name 从远程仓库抓取数据
    - git push remote-name branch-name 把本地的master分支推送到origin服务器上。
    - git remote rename old-remote-name new-remote-name 修改某个远程仓库在本地的简称
    - git remote rm remote-name 删除对应的远程仓库(远程仓库中的数据不会参数，仅仅解除本地与服务器的链接，相当与git remote add的反操作)
    - git checkout --track origin/serverfix 当你克隆一个远程仓库的时候，实际上是将远程仓库的origin/master分支合并到本地的master分支上，而远程仓库的其他分支并没有被下载下来，可以通过该命令将远程仓库的某个分支下载下来。

## git官方文档中文版  

[git官方文档](https://git-scm.com/book/zh/v2)
