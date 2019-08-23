#### 增删查看文件、文件夹
- mkdir xx 
- rmdir xx
- echo ''>>xx.txt
- rm -rf xx
- ls -a



#### 几个区
1. Workspace：工作区
2. Index/Stage：暂存区
3. Repository: 本地仓库
4. Remote：远程仓库



#### git的设置文件 .gitconfig
- git config --[local || global || system] user.name 'xuwang'
- git config --[local || global || system] user.name 'xxx'

- git config --[local || global || system] --unset user.name
- git config --[local || global || system] --unset user.name

- git config --list




#### 新建代码库
1. git init 
2. git init [object-name]
3. git clone [url]




#### 添加文件
1. git add [file1] [file2] ... //添加指定文件到暂存区

2. git add . //添加当前目录的所有文件到暂存区




#### 删除文件
1. git rm [file1] [file2] ... //删除工作区文件，并且将这次删除放入暂存区

2. git rm --cached [file1] [file2] ... //停止追踪指定文件，但该文件会保留在工作区（还未提交过的文件）

3. git mv [file-original] [file-renamed] //改名文件，并且将这个改名放入暂存区（被追踪文件）




#### 代码提交
1. git commit [file1] [file2] ... -m [message] //提交暂存区的指定文件到仓库区

2. git commit --amend//（按i键开始编辑、按Esc退出编辑、:wq+Enter完成编辑）



#### 分支
1. git branch //列出所有本地分支

2. git branch -r //列出所有远程分支

3. git branch -a //列出所有本地分支和远程分支

4. git branch [branch-name] //新建一个分支，但依然停留在当前分支

5. git checkout [branch-name] //切换到指定分支，并更新工作区

6. git checkout -b [branch] //新建一个分支，并切换到该分支

7. git branch -d [branch-name] //删除分支 （git branch -D [branch-name]慎用-D）

8. git push origin --delete [branch-name]     git branch -dr [remote/branch] //删除远程分支 



#### 分支合并
1. git fetch 

2. git merge

3. git rebase

4. git branch --merged //已经合并进来了的分支

5. git branch --no-merged //还未合并进来的分支




#### 查看信息
1. git status //显示有变更的文件

2. git log //显示当前分支的版本历史

3. git diff xx..xx //显示暂存区和工作区的差异



#### 远程同步
1. git remote -v //显示所有远程仓库


2. git remote add [shortname] [url] //增加一个新的远程仓库，并命名


3. git remote rm origin //删除指定远程仓库


4. git pull [remote] [branch] //取回远程仓库的变化，并与本地分支合并


5. git push [remote] [branch] //上传本地指定分支到远程仓库


6. git push [remote] --force //强行推送当前分支到远程仓库，即使有冲突


7. git push [remote] --all //推送所有分支到远程仓库



#### 撤销
1. git checkout [file] //恢复暂存区的指定文件到工作区

2. git checkout . //恢复暂存区的所有文件到工作区

3. git checkout [commit] [file] //恢复某个commit的指定文件到暂存区和工作区

4. git checkout [commit] . //恢复某个commit的所有文件到暂存区和工作区


5. git reset [file] //重置暂存区的指定文件，与上一次commit保持一致，但工作区不变

6. git reset [commit] //重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变

7. git reset --hard //重置暂存区与工作区，与上一次commit保持一致

8. git reset --hard [commit] //重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致


#### 暂时将未提交的变化移除，稍后再移入（前提该文件为被追踪文件）
1. git stash //添加

2. git stash apply @stash{0}

3. git stash drop @stash{0}

4. git stash list //查看




#### 其他
1. git archive //生成一个可供发布的压缩包

[以上来自阮一峰的网络日志：http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)