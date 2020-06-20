#### 员工A初始化一个工程

0. git init config

   ```shell
   git config --global user.name "thonsun"
   git config --global user.email "thonsun@163.com"
   
   # ~/.gitconfig
   ```

   

1. init local workspace

   ```shell
   mkdir gitbook & cd gitbook
   git init
   ```

   

2. coding

   ```shell
   touch REAME.md
   git status # 查看本地workspace,index(stage暂存区),repository 状态
   git add REAME.md # add to stage
   git status
   ```

   

3. commit to local repository

   ```shell
   git commit -m "init REAME.md"
   git commit -am "short msg"
   git commit
   git commit -a
   
   git rm --cached <file>...
   ```

   

4. check commit history

   ```shell
   # 全部提交历史
   git log
   git log filename
   
   # 首行提交记录
   git log --pretty=short
   
   # 文件级改动记录
   git log -p
   git log -p filename
   
   # 具体文件与哪个版本提交的差异
   git diff filename commit-id
   
   ```

   

5. branch and workflow

   ```shell
   # 显示本地与远程分支
   git branch
   git branch -a 
   
   # 本地首次添加分支
   ## add branch
   git branch branchNmae
   ## chechout branch
   git checkout branchName
   ## 切换到新建的分支，本地workspace 目录改变，on branchName:chage git add|commit 只影响到当前branch
   ## 上面命令可以等价于 git checkout -b branchName
   
   # 分支间切换
   git checkout master
   git checkout -
   ## 当前分支的workspace 要commit 才能切换分支
   
   # 合并branchName 到 master
   ## git checkout master
   git merge --no-ff branchName
   ## git log -graph 可以看到
   ```

   

6. revert or goto commit history version

   ```shell
   git reset --hard commit-id (commit id 可前可后)
   # branch的新增 文件内容没有改变，本地repo 不变
   
   # 查看所有git 提交历史
   git reflog 
   # git log可以显示所有提交过的版本信息，不包括已经被删除的commit 记录和reset 的操作git reflog是显示所有的操作记录，包括提交，回退的操作。 一般用来找出操作记录中的版本号，进行回退。 git reflog常用于恢复本地的错误操作。
   
   # 冲突解决
   vim filename
   git add .
   git commit -am "solved merged conflict"
   ```

   

7. rewrite last commit message

   场景：上次的提交commit message 不够好

   如在合并分支出现的冲突解决后提交的commit message,应该为 Merge branch

   ```shell
   git commit --amend
   ```

   

8. 合并两次修改为新一次提交

   将当前分支的头两次commit history 合并，如一些提交内容错误的修改应该一次的修改

   ```shell
   # coding...
   git commit -am "ADD with wrong msg"
   #coding 
   git commit -am "Fix typo"
   
   git rebase -i HEAD~2
   ## pick 改 fixup
   
   # rebase 的使用
   # Rebase f6f90f3..8887a67 onto f6f90f3 (3 commands)
   #
   # Commands:
   # p, pick <commit> = use commit
   # r, reword <commit> = use commit, but edit the commit message
   # e, edit <commit> = use commit, but stop for amending
   # s, squash <commit> = use commit, but meld into previous commit
   # f, fixup <commit> = like "squash", but discard this commit's log message
   # x, exec <command> = run command (the rest of the line) using shell
   # b, break = stop here (continue rebase later with 'git rebase --continue')
   # d, drop <commit> = remove commit
   # l, label <label> = label current HEAD with a name
   # t, reset <label> = reset HEAD to a label
   # m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
   # .       create a merge commit using the original merge commit's
   # .       message (or the oneline, if no original merge commit was
   # .       specified). Use -c <commit> to reword the commit message.
   #
   # These lines can be re-ordered; they are executed from top to bottom.
   #
   # If you remove a line here THAT COMMIT WILL BE LOST.
   #
   # However, if you remove everything, the rebase will be aborted.
   #
   # Note that empty commits are commented out
   ```

   

9. push local repo to remote git repo

   远程repo 方便多人协作，多地协作

   ```shell
   git remote add origin xxx
   git remote -v
   git push -u origin master
   
   # 将本地不同的分支依次提交
   git checkout branchName
   git push origin branchName
   ```

   -u 参数可以在推送的同时，将 origin 仓库的 master 分 支设置为本地仓库当前分支的 upstream（上游）。添加了这个参数，将来 运行 git pull 命令从远程仓库获取内容时，本地仓库的这个分支就可 以直接从 origin 的 master 分支获取内容，省去了另外添加参数的麻烦

   git add license 

   两个没有共同commit local repo ，remote repo 拒绝merge

   `git pull origin master --allow-unrelated-histories`

#### 多人协作&多地开发

workerB 加入workerA 开发

1. clone remote repo

   

2. checkout new branchName

   或者基于某个分支branchName1 进行开发

   ```shell
   # 同步远程repo 指定分支到本地
   # 会更改本地workspace 内容，即git pull,git checkout branchName1
   
   git checkout -b branchName1 origin/branchName1
   
   # 在branchName1 的基础上新建属于自己branchName
   ```

   -b branchName1 :本地创建一个分支branchName1

   后面的 origin/branchName1 :指定本地创建分支基于远程指定分支内容

   

3. coding

   

4. git push origin branchName

   

5. pull and merge

   将远程repo 最新的修改同步到本地repo:

   ```shell
   git pull origin xxxx_branch
   ```

   

6. 代码冲突解决

   ```shell
   # 在本地新建一个tmp分支，并将远程origin仓库的master分支代码下载到本地tmp分支
   git fetch origin branch_name:tmp 
   
   # 来比较本地代码与刚刚从远程下载下来的代码的区别
   git diff tmp 
   
   # 合并tmp分支到本地的master分支
   git merge tmp
   # 不同分支的合并才会出现代码冲突，需要手动解决
   On branch test_merge_usage
   You have unmerged paths.
     (fix conflicts and run "git commit")
     (use "git merge --abort" to abort the merge)
   
   Unmerged paths:
     (use "git add <file>..." to mark resolution)
   	both modified:   README.md
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   # 即手动解决了冲突：1.git add file -> 2.git commit -> git push origin master
   
   # 如果不想保留tmp分支 可以用这步删除
   git branch -d tmp
   ```

   

