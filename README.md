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
   git reset --hard commit-id
   # branch 新增内容没有改变，本地repo 不变
   
   # 查看所有git 提交历史
   git reflog 
   
   # 冲突解决
   vim filename
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
   # 会更改本地workspace 内容，即git pull | git checkout branchName1
   
   git checkout -b branchName1 origin/branchName1
   
   # 在branchName1 的基础上新建属于自己branchName
   ```

   -b branchName1 :本地创建一个分支branchName1

   后面的 origin/branchName1 :指定本地创建分支基于远程指定分支内容

   

3. coding

   

4. git push origin branchName

   

5. pull merge
