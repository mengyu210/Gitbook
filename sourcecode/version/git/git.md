# Git
## Git中代码冲突的解决方式
   ### 冲突1：
   当你commit以后，在执行git pull --rebase的时候出现冲突，请按如下步骤解决：
   - 找到冲突文件，解决冲突
   - 执行git add xxx（xxx为冲突文件全路径）
   - 执行git rebase --continue
   - 执行git pull --rebase
   - 执行git push
  
   ### 冲突2：
   当你本地有修改的时候，你执行了git stash，然后又从服务器上pull了最新代码（git pull --rebase），出现了冲突，请按如下方式解决：
   -  找到冲突文件，解决冲突
   -  执行git add xxx（xxx为冲突文件全路径）
   -  git commit
   -  git pull --rebase
   -  git push
## Git最常用命令集合：
```
git clone ssh://xxx/xx.git                下载git库
git branch -a                             查看所有分支
git checkout xxx                          切换到xxx分支
git branch                                查看当前所属分支
git submodule init                         子模块初始化
git submodule update                       子模块更新
git status                                 查看本地代码状态
git pull --rebase                          从服务器拉取最新代码
git add xxx                                将xxx文件加入待提交列表
git add .                                  将本地所有有修改文件加入待提交列表
git commit                                 提交代码到新任务
git commit --amend                          提交代码到已有任务
git push xxx                                将本地修改push到git服务器
git reset HEAD^                             将本地状态恢复至代码未提交之前
```