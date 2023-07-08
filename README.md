# 练习git
- git clone 'xxx.git' 克隆远程仓库地址
  - 添加 .git目录，该目录是 本地仓库，.git所在的根目录是工作目录
- git status 查看当前工作目录状态
  - 告知了当前所在分支
  - 和远程仓库相同分支是否有差距
  - 未跟踪的文件 
  - 已经跟踪但是后面又修改了 (前面使用 modified修饰)
  - 已经暂存文件 （前面使用 modified | new file 修饰）
    - 暂存区位于 .git目录下的index文件中
- git add 将未跟踪文件进行跟踪
- git commit 将暂存区文件放入本地仓库
- git push 提交到远程仓库
  - 两边都修改，然后push，后面push会失败
  - 这是因为git的push是使用了本地仓库的commit记录去添加的，如果远端仓库没有改本地commit则失败
- git pull 拉取远程代码并合并
  - 相当于 git fetch + git merge 
  - 如果自己代码有修改，拉取远程仓库代码，则终止

## head、main与branch
- head是唯一的，可以指向commit，也可以指向branch
- branch只是一个commit的引用
  - git branch xxx  创建新分支
  - git checkout xxx  切换分支
  - git checkout -b xxx 合并上面两个操作
  - git checkout -d xxx 删除分支（当前Head不能指向被删除分支）