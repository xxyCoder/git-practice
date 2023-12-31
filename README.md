# 练习git
- git clone 'xxx.git' 克隆远程仓库地址
  - 添加 .git目录，该目录是 本地仓库，.git所在的根目录是工作目录
  - git clone 'xxx.git' newName 指定新目录名称

- git config 配置git，每次git提交都会使用到以下信息
  - git config --global user.name xxx
  - git config --global user.email xxx
  - git config <key> 检查某一项
  - git config --global alias cmd 给cmd命令起别名

- git init 初始化仓库，产生一个 .git子目录

- git log 查看历史记录
  - git log -p 查看详细的历史记录
  - git log --stat 查看简要统计
  - git log --graph 查看各个分支之间关系
  
- git status 查看当前工作目录哪些文件处于什么状态
  - 告知了当前所在分支
  - 和远程仓库相同分支是否有差距
  - 未跟踪的文件 
  - 已经跟踪但是后面又修改了 (前面使用 modified修饰)
  - 已经暂存文件 （前面使用 modified | new file 修饰）
    - 暂存区位于 .git目录下的index文件中

# 对比文件差异（基于同一个文件对比）
- git diff 工作区有改动，但暂存区没有，则与最后一次commit-id做对比；如果暂存区有东西，则与暂存区进行对比
- git diff --staged 比较已经暂存文件和最后一次提交的文件之间的差异  
- git diff head 显示工作区和暂存区与最后一次commid-id之间所有不相同文件的修改
- git diff <commit-id2> <commit-id2> 比较两个commid-id之间的差异
- git diff <branch-name1> <branch-name2> 比较两个分支最后的commit内容差别
 
- git add 将未跟踪文件进行跟踪
- git commit 将暂存区文件放入本地仓库
- git rm 将暂存区中文件移除，连带从工作区中也移除
  - 如果要移除之前修改过或已经放入暂存区的文件，必须加 -f 强制性删除
  - 如果希望保存即不在工作区中移除， 使用 --cache
- git push 提交到远程仓库
  - 两边都修改，然后push，后面push会失败
  - 这是因为git的push是使用了本地仓库的commit记录去添加的，如果远端仓库没有改本地commit则失败
  
- git pull 拉取远程代码并合并
  - 相当于 git fetch + git merge 
  - 如果自己代码有修改，拉取远程仓库代码，则终止
  
- git show commitID 查看某个具体commit的改动内容

## head、main与branch
- head是唯一的，可以指向commit，也可以指向branch
- branch只是一个commit的引用
  - git branch xxx  创建新分支
  - git checkout xxx  切换分支
  - git checkout -b xxx 合并上面两个操作
  - git branch -d xxx 删除分支（当前Head不能指向被删除分支）
  - git branch -D xxx 强制删除，无论有无未push的代码
    - 删除分支也只是删除引用commit的指针，不过一个commit不在任何一个branch的路径上，在一定时间之后会被git回收
  - git branch --merged 查看哪些已经合并到当前分支
  - git branch --no-merged 查看哪些没有合并到当前分支

- git merge xxx 合并某个commit到当前commit中
  - fast-forward ，当合并的分支是当前分支的直接后继
  - 合并，当合并的分支不是当前分支的直接后继，会将合并的结果做一个commit，并指向它
    - 使用 git status观察处于冲突且未合并的文件
- git rebase xxx 将当前分支的提交在xxx中重新提交一遍
  - 之后要切回到xxx分支进行Merge，将xxx移到最新
  - git rebase --onto x1 x2 x3 将x3中的commit除去x2中的commit，剩余的commit（即找到x3从x2开始分歧之后的commit）合并到x1中
- git cherry-pick commid|brancName 将指定的commit或其他分支的最新提交应用于当前分支
  - -e --edit 打开外部编辑器，编辑提交信息
  - -n --no-commit  只更新工作区和暂存区，不产生新的提交
  - -x  添加注释
  - 产生冲突并解决之后 
    - 将修改的文件添入暂存区， 然后继续执行(git cherry-pick --continue)
    - --abort 发生冲突后，放弃合并
    - --quit  发生冲突后，退出cherry-pick，但是不回到操作之前的样子

- git commit --amend 将当前commit和暂存区的内容合并创建一个新的commit，用这个新的commit 替换当前commit
- git rebase -i 交互式rebase，可以指定要rebase的commit链中每一个commit是否需要进一步修改
  - git rebase -i HEAD^^ ^的个数表示commit回溯的个数
  - git rebase -i HEAD~number 表示commit回缩的number个数

- git remote add 仓库名 xxx 添加远程仓库
- git remote show 仓库名 xxx 查看某个远程仓库
- git remote rename 仓库名 oldName newName 更改某个远程仓库的别名
- git remote remove 仓库名 xxx 删除某个远程仓库

- git fetch xxx 拉取远程仓库所没有的信息

# 撤销
## 撤销已经修改但是未暂存
- git checkout -- <filename>
- git checkout . 全部
- git restore <filename>

## 已经暂存，未提交，将暂存区内容还原到工作区
- git reset head 全部
- git reset head -- <filename> 
- git restore --staged <filename>

## 已提交，未推送
- git reset head^ 将提交的还原到工作区
- git reset <commit-id> 将提交还原到工作区
- git reset --soft head 将推送还原到暂存区
- git revert <commit-id> 生成一个新的commit，在将指定的commit内容从分支中删除（不会回到暂存区也不会回到工作区）

## 将提交、暂存的、修改的全部清空
- git reset --hard head
- git reset --hard <commit-id>

## 已经推送
- 回滚本地仓库，强制推送覆盖远程仓库
1. 
  - git reset --soft head^
  - git push --force
2.
  - git revert head
  - git push

## 撤销 撤销修改
- git reflog 查看撤销之前的commit id
- git reset <commit-id>即可
- 不过要尽快，赶在git清理commit-id之前还可以找回

# 合并分支
- git merge <branch-name> 将指定分支合并到当前分支  
  - 合并是新增一个commid，将两个分支联系起来
- git rebase <branch-name> 将当前分支移到指定分支或commid-id
  - 会将整个分支移动到另一个分支

# 拉取远程分支代码
- 修改后要先commit之后才能git pull，有冲突解决冲突后然后再次commit
- 推荐先git pull后再修改代码

# git知识点
- git中所有的数据都存储前计算校验和（使用SHA-1计算内容作为哈希值），然后以校验和引用