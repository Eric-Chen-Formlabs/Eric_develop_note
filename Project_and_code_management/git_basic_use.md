# 基本概述  
https://www.cnblogs.com/jamiechoo/articles/18408791


### Fork and Pull Request
- 先在github上fork一个项目，然后clone到本地
- 然后添加上游仓库 git remote add upstream xxxx.git
- 然后创建分支： git checkout -b xxx/xxxxx
- 在开始修改前同步上游更新，git fetch upstream, git checkout master, git merge upstream/master, git push origin master， git checkout yourbranch, git merge master  
- 然后开始修改，修改完后，git add . , git commit -m  'feat:xxxxx',  git push origin xxx/xxxx
- 然后回到自己fork的界面，创建pull request



### clone


克隆远端仓库到本地
git clone <git url>

克隆远端仓库到本地，并同时切换到指定分支 branch1
git clone <git url> -b branch1

克隆远端仓库到本地并指定本地仓库的文件夹名称为 my-project
git clone <git url> my-project

### 初始化仓库
git init
git add .
git --global user.name "Your Name"
git --global user.email "Your Email"
git commit -m "first commit"  
git remote add origin <git url>
git push -u origin master

### 版本切换  
git branch -a 所有版本  
git branch -vv 远程与本地关联  
git checkout test 本地切换到test分支

切换到已有的本地分支 branch1
git checkout branch1

切换到远程分支 branch1
git checkout origin/branch1

基于当前本地分支创建一个新分支 branch2，并切换至 branch2
git checkout -b branch2

基于远程分支 branch1 创建一个新分支 branch2，并切换至 branch2
git checkout origin/branch1 -b branch2

当前创建的 branch2 关联的上游分支是 origin/branch1，所以 push 时需要如下命令关联到远程 branch2
git push --set-upstream origin branch2

撤销工作区 file 内容的修改。危险操作，谨慎使用
git checkout -- <file>

撤销工作区所有内容的修改。危险操作，谨慎使用
git checkout .


### 提交  

将所有修改的文件都提交到暂存区
git add .

将修改的文件中的指定的文件 a.js 和 b.js 提交到暂存区
git add ./a.js ./b.js

将 js 文件夹下修改的内容提交到暂存区
git add ./js

把当前本地的分支推送到远程dev-CY分支（需要关联）
git push origin HEAD:dev-CY 

将当前本地分支 branch1 内容推送到远程分支 origin/branch1
git push

若当前本地分支 branch1，没有对应的远程分支 origin/branch1，需要为推送当前分支并建立与远程上游的跟踪
git push --set-upstream origin branch1

强制提交
例如用在代码回滚后内容
git push -f


### 拉取
若拉取并合并的远程分支和当前本地分支名称一致

例如当前本地分支为 branch1，要拉取并合并 origin/branch1，则直接执行：
git pull

若拉取并合并的远程分支和当前本地分支名称不一致
git pull <远程主机名> <分支名>

例如当前本地分支为 branch2，要拉取并合并 origin/branch1，则执行：
git pull git@github.com:zh-lx/git-practice.git branch1

使用 rebase 模式进行合并
git pull --rebase

### 回撤提交

将 a.js 文件取消缓存（取消 add 操作，不改变文件内容）
git reset --staged a.js

将所有文件取消缓存
git reset --staged .

取消某次 commit 内容，但是保留 commit 记录
git revert <commit-sha>

### 缓存代码
把本地的改动缓存起来
git stash

缓存代码时添加备注，便于查找。强烈推荐
git stash save "message"

查看缓存记录
eg: stash@{0}: On feat-1.1: 活动功能
git stash list

取出上一次缓存的代码，并删除这次缓存
git stash pop
取出 index 为2缓存代码，并删除这次缓存，index 为对应 git stash list 所列出来的
git stash pop stash@{2}

取出上一次缓存的代码，但不删除这次缓存
stash apply
取出 index 为2缓存代码，但不删除缓存
git stash apply stash@{2}

清除某次的缓存
git stash drop stash@{n}

清除所有缓存
git stash clear


### 放弃修改
git restore <file>           # 放弃单个文件的修改
git restore .                # 放弃所有文件的修改
git checkout -- <file>       # 旧方式：放弃单个文件修改

git restore --staged <file>  # 取消暂存（修改保留在工作区）
git reset HEAD <file>        # 旧方式：取消暂存

### 修改查看远程等
git remote set-url origin xxx(为改名后的新地址)
git remote -v


### 查看工作区状态、日志

查看当前工作区暂存区变动
git status 

以概要形式查看工作区暂存区变动
git status -s 

查询工作区中是否有 stash 缓存
git status --show-stash


显示 commit 日志
git log

以简要模式显示 commit 日志
git log --oneline

显示最近 n 次的 commit 日志
git log -n

显示 commit 及分支的图形化变更
git log --graph --decorate




# 常用步骤流程

### 添加别人的仓库或者分支为远程,并创建本地分支
git remote -v 
git remote add xxxxname githublink
git fetch xxxxname
git checkout -b mordor-main zsombor/main

### 拉取远程最新的更改
git fetch upstream
git checkout master
git merge upstream/master
git push origin master


git checkout dev/clerk-support-sanic


git merge master


git push origin dev/clerk-support-sanic


### 查看某个分支有没有未提交的修改

### 对比不同branch和commit的文件差异

git diff sanic-clerk-setup -- Src/main.c

git diff branch1..branch2 -- Src/main.c

git diff commit1 commit2 -- Src/main.c


### 将某个分支的某个文件夹或者文件的修改应用到当前分支上

git checkout targetbranch -- xxxx/ssss/xxxx


### 拉取某个PR到本地

git fetch origin pull/12635/head:the-name-you-want
git checkout the-name-you-want

### 暂存当前，回退到历史，然后再回到当前

步骤1: 暂存当前所有修改（带描述）
git stash save "new modified codes"

步骤2: 查看暂存列表（确认已保存）
git stash list
输出示例: stash@{0}: On sanic-clerk-setup: new modified codes

步骤3: 回退到指定commit
git checkout 8d1d1cbffbc

步骤4: 查看/测试历史版本代码
... 做你需要的工作 ...

步骤5: 切回原分支
git switch sanic-clerk-setup
或
git checkout -

步骤6: 恢复暂存的修改
git stash pop
如果有冲突，解决后删除stash: git stash drop


### 暂存当前，切换到其它分支

步骤1: 暂存当前修改
git stash save "PWM调试中的临时修改"

步骤2: 切换到其他分支
git switch feature/uart-test

步骤3: 在新分支上工作
git add .
git commit -m "修复UART bug"

步骤4: 切回原分支
git switch sanic-clerk-setup

步骤5: 恢复之前的修改
git stash pop

### 暂存当前，回退到历史，修复bug，创建修复bug分支，然后上传到仓库，然后切换回当前分支，继续开发

步骤1: 暂存当前工作区的修改
git stash save "当前开发中的功能 - 未完成"

步骤2: 查看提交历史，找到需要修复的提交
git log --oneline -10

步骤3: 回退到出现 bug 的历史提交
git checkout <commit-hash>
例如: git checkout 8d1d1cbffbc

步骤4: 修复 bug（手动修改代码文件）
... 编辑文件修复 bug ...

步骤5: 基于当前位置创建 bug 修复分支
git checkout -b fix/pwm-complementary-disable

步骤6: 添加修改的文件到暂存区
git add Src/main.c sanic_bsp/sanic_pwm.c sanic_bsp/sanic_pwm.h

步骤7: 提交 bug 修复
git commit -m "Fix: Disable PWM complementary output on PA9

- PA8 (CH0): Single-ended PWM output in TOGGLE mode
- PA9 (CH1): Force LOW during transmission (not complementary)
- PA8/PA9: High-Z input during receive (no pull-up per requirement)
- Align timing with diesel project (BLANK_TIME_US=150, RINGOUT_TIME_US=125)
- Explicitly disable CH1 output state to prevent unwanted waveform"

步骤8: 推送 bug 修复分支到远程仓库
git push -u origin fix/pwm-complementary-disable

步骤9: 切换回原来的开发分支
git checkout sanic-clerk-setup

步骤10: 恢复之前暂存的修改
git stash pop

步骤11: 继续开发工作
... 继续编写代码 ...


### 在历史提交中修复了bug，创建了bug分支，在当前最新仓库里合并修复的bug

步骤1: 查看当前所有分支（确认 bug 修复分支存在）
git branch -a

步骤2: 确保在最新的开发分支上
git checkout sanic-clerk-setup

步骤3: 拉取最新的远程更新（如果有团队协作）
git pull origin sanic-clerk-setup

步骤4: 查看 bug 修复分支的提交内容（可选，用于确认）
git log fix/pwm-complementary-disable --oneline -5

步骤5: 合并 bug 修复分支到当前开发分支
git merge fix/pwm-complementary-disable -m "Merge bug fix: PWM complementary output disabled"

如果希望保持线性历史，可以使用 rebase:
git rebase fix/pwm-complementary-disable

步骤6: 解决冲突（如果有）
git status  # 查看冲突文件
... 手动解决冲突 ...
git add <resolved-files>
git commit  # 完成合并

步骤7: 推送合并后的开发分支到远程
git push origin sanic-clerk-setup

步骤8: 删除本地 bug 修复分支（可选）
git branch -d fix/pwm-complementary-disable

步骤9: 删除远程 bug 修复分支（可选，如果确定不再需要）
git push origin --delete fix/pwm-complementary-disable

步骤10: 查看合并后的提交历史
git log --oneline --graph -10