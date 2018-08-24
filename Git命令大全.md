# Git命令大全
## 1、配置Git 
```
## 安装 Git 之后，你要做的第一件事情就是去配置你的名字和邮箱，因为每一次提交都需要这些信息
git config --global user.name "your name"
git config --global user.email "your email"
## 获取Git配置信息
git config --list
## 生成SSH
ssh-keygen -t rsa -C "youremail@example.com"
```
## 2、创建版本库和提交代码
```
## 初始化一个git仓库
git init
## 关联(添加)远程库地址（可关联多个）git remote add 别名 地址
git remote add origin http://host/repository/myproject.git 
## 查看仓库状态
git status
## 将所有修改添加到暂存区
git add .
## Ant风格添加修改
git add *
##  将以Controller结尾的文件的所有修改添加到暂存区
git add *Controller 
## 将所有以Hello开头的文件的修改添加到暂存区 例如:HelloWorld.txt,Hello.java
git add Hello*  
## 将以Hello开头后面只有一位的文件的修改提交到暂存区 例如:Hello1.txt,HelloA.java      
## 如果是HelloGit.txt或者Hello.java是不会被添加的
git add Hello?
## 将暂存区的修改提交到仓库
git commit -m "comment" 
## 将工作区的修改提交到仓库，相当于git add . 与 git commit -m "comment"的合体
git commit –am "comment" 或 git commit –a –m "comment"
```
## 3、本地和远程的同步
```
## 查看远程库地址信息
git remote -v
## 查看远程库origin状态
git remote show origin
## 修改关联的远程库地址 git remote set-url 别名 地址
git remote set-url origin http://192.168.1.161/ddc/ddcsz.git
## 删除关联的远程库地址
git remote rm origin
## 推送本地库代码到远程库
git push -u origin master //推送本地代码到origin库的master分支，origin为关联地址时写的别名
## 从远程的origin仓库的master分支下载代码到本地的origin master
git fetch origin master
## 从远程origin仓库的master分支下载代码到本地temp分支，如果temp分支不存在则会新建一个；不会自动合并
git fetch origin master:temp
## 如果不加参数，则取回所有分支的更新
git fetch
## 取回远程next分支，与本地master分支合并；相当于先git fetch再git merge
git pull origin next:master
## 如果远程next分支要与当前分支合并，则可以省略冒号部分
git pull origin next
## 当前分支自动与唯一一个追踪分支进行合并
git pull
## 手动建立追踪(关联)关系：指定本地当前所在分支追踪远程next分支
git branch --set-upstream-to origin/next
```
## 4、分支管理
```
## 新建分支
git branch dev
## 重命名分支
git branch -m old_local_branch_name new_local_branch_name
## 切换到分支
git checkout dev
## 新建+切换分支
git checkout -b dev
## 新建+切换分支+关联远程分支
git checkout -b dev origin/dev
## 推送本地分支到远程库
git push origin dev
## Fast forward模式合并dev分支到当前分支,不记录本次操作
git merge dev
## Fast forward模式合并远程dev分支到当前分支
git merge origin/dev
## 禁用Fast forward模式，合并dev分支到当前分支，幷记录本次操作;如果不使用-m参数
git merge --no-ff -m “Merge branch dev” dev
## 查看本地分支
git branch
## 查看遠程分支
git branch -r
## 查看包括本地和远程所有的分支
git branch -a
## 查看各个分支最后一个提交对象的信息
git branch -v
## 查看各个分支对应的远程分支及最后一个提交对象的信息
git branch -vv
## 查看哪些分支被幷入当前分支
git branch --merged
## 查看尚未与当前分支合并的分支
git branch --no-merged
## 删除本地dev分支
git branch -d dev
## 强制删除本地dev分支
git branch -D dev
## 删除本地对应的远程分支
git branch -r -d origin/dev
## 远程删除git服务器上的分支
git push origin -d dev
git push origin --delete dev
## 将当前分支的修改打成补丁，以develop最近一次提交作为基础，然后将补丁运用上去
git rebase develop
## 放弃本次rebase过程，回到初始状态
git rebase --abort
## 跳过本次补丁运用
git rebase --skip
## 继续运用补丁
git rebase --continue
```
## 5、查看日志
```
## 以默认格式输出日志
git log
## 将每条日志输出为一行
git log --oneline
## 指定显示最近多少条日志 git log -[length]
git log --oneline -2
## 指定跳过前几条日志 git log --skip=[skip]
git log --skip=1 -2 --oneline
## 显示更多提交信息，包括提交ID，文件树ID，父提交ID，作者和提交者
git log –pretty=raw
## 显示提交的具体改动记录，相当于多次使用git show [commit_id]
git log -p
## 仅查看哪些文件有改动
git log --stat
## 比较本地仓库和远程仓库的区别
git log -p master.. origin/master
## 绘制提交线索，有合并也会显示
git log --graph --oneline
## 显示合并过程，简略提交信息
git log --graph --pretty=oneline --abbrev-commit
## 显示一些相关的信息，如HEAD、分支名、tag名等
git log --decorate --oneline
## 显示每次提交对应的文件改动
git log --name-status --oneline
## 通过作者搜索提交日志,yourname可以包含通配符
git log --author yourname
## 通过提交关键字搜索日志
git log --grep keywords
## 以上两者组合使用搜索日志
git log --author yourname --grep keywords
## 查看最近提交的tag
git describe
## 查看各种对象的详细情况
git show [commit_id|tag_name|branch_name|...]
## 查看某个文件的历史修改情况
git blame [file_path]
## 查看分支操作记录
git reflog
```
## 6、标签(tag)的使用
```
## 创建轻量标签：不需要传递参数，指定标签名即可
git tag v1.0.0
## 创建附注标签：参数-a即annotated的缩写，指定标签类型，后附标签名。参数m指定标签说
## 明，说明信息会保存在标签对象中
git tag -a v1.0.0 -m "标签说明"
## 列出当前仓库的所有标签
git tag
## 列出符合模式的标签
git tag -l 'v1.0.*'
## 查看标签版本信息
git show v1.0.0
## 切换到标签
git checkout v1.0.0
## 删除本地标签 
git tag -d v1.0.0 
## 删除远程库标签
git push origin :refs/tags/v1.0.0
## 给指定的commit打标签
git tag -a v1.0.1 [commit_id] -m "打标签测试"
## 将指定标签提交到git服务器
git push origin v1.0.0
## 将本地所有标签一次性提交到git服务器
git push origin –-tags
```
## 7、比较修改文件
```
## 工作区与暂存区比较
git diff filepath
## 工作区与HEAD(当前版本)比较
git diff  HEAD -- filepath
## 暂存区与HEAD比较
git diff --staged filepath 或者 git diff --cached filepath
## 当前分支的文件与branchName 分支的文件进行比较
git diff branchName filepath
## 两个提交版本之间的比较 
git diff commitId1 commitId2
## 与某一次提交的某个文件夹或者文件进行比较
git diff commitId filepath 
## 两个版本的src文件夹或者文件的比较
git diff commitId1 commitId2 src

# 使用 git diff 打补丁
## patch的命名是随意的，不加其他参数时作用是当我们希望将本仓库工作区的修改拷贝一份到其他机器上使用，
## 但是修改的文件比较多，拷贝量比较大，此时我们可以将修改的代码做成补丁，之后在其他机器上对应目录下使用 ## git apply patch 将补丁打上即可
git diff > patch
## 运用补丁
git apply patch
## 两个commit间的修改（包含两个commit）（带提交信息）
git format-patch commitId1 commitId2
## 单个commit（带提交信息）
git format-patch -1 commitId
## 从某commit以来的修改（不包含该commit）
git format-patch commitId
## 生成xxx.rej冲突文件，不带提交信息，需要重新提交
git apply --reject *.patch
```
## 8、撤销修改和版本回退
```
## 修改上次提交信息
git commit --amend
## 撤销暂存区的修改，重新放回工作区
git reset HEAD [file_path]
## 丢弃工作区的修改，恢复到修改前的版本状态
git checkout -- [file_path]
## 撤销所有修改文件
git reset --hard
## 撤销已提交到本地仓库但是还未推送到远程的文件
git reset --hard origin/master
## 回退到上一个版本(注：git中的HEAD指当前版本，上一个加上^表示，上上个用两个^，3个
## 用HEAD~3，以此类推)
git reset --hard HEAD^
## 回退到指定版本
git reset --hard [commit_id]
## 以上命令hard参数可以被替换为soft和mixed参数
git reset --hard //版本和状态一起回退，并删除回退点之前的信息，回退的比较彻底干净
git reset --soft //版本和状态一起回退，但是会保留回退点之前的信息
git reset --mixed //版本回退，但是会保留回退点之前的状态和信息，为默认操作
```
## 9、储藏(stash)
```
## 储藏当前所有修改，让工作区变得干净
git stash save [description]
## 查看现有的stash储藏列表
git stash list
## 重新应用指定储藏,不加参数则默认第一个
git stash apply [stash@{id}] //id可通过list命令查看
## 重新应用第一个stash,并删除
git stash pop
## 重新应用指定stash，并删除
git stash pop [stash@{id}]
## 清空stash列表
git stash clear
## 删除指定stash
git stash drop [stash@{id}]
## 基于stash创建分支，创建成功则会删除stash
git stash branch [branch_name]
```
## 10、删除
```
## 更新远程库对象时删除没用的对象
git fetch origin --prune
## 删除远程库没用的对象
git remote prune origin
## 删除未跟踪(没有添加到暂存区)的文件
git clean -f [file_path]
## 删除未跟踪的目录及文件
git clean -df [dir_path]
## 删除所有未跟踪的目录及文件
git clean -xdf
## 显示即将要删除的目录和文件
git clean -n
## 移除项目的版本控制(删除.git文件)
rm -rf .git
```
## 11、跟踪处理
```
## 忽略已跟踪的文件
git update-index --assume-unchanged [file_path]
## 恢复跟踪
git update-index --no-assume-unchanged [file_path]
## 取消某个文件跟踪，使之成为未跟踪状态
git rm -f --cached [file_path]　
## 取消某个文件目录跟踪，使之成为未跟踪状态
git rm -r --cached [dir_path]

```