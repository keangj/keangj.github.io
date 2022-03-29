# GIT



### 比较差异

``` sh
git diff # 比较工作区与暂存区的差异
git diff --cached # 比较暂存区与版本区的差异
git diff master # 比较工作区与版本区的差异
```

### 基本

``` sh
git init # 初始化
git status # 查看当前仓库状态信息
git add <file> # 将文件加入暂存区
git reset HEAD <file> # 暂存区与版本区保持一致
git reset --hard <version> # 恢复版本区指定版本的内容到工作区
git checkout <file>	# 暂存区（或者版本区）覆盖工作区的内容
git rm <file> --cached # 删除暂存区文件
git commit -a -m '' # git add . git commit -m ''
git refolg # 查看引用版本号
git log	# 查看日志
git reflog. # 
git log --oneline -- graph # 查看历史操作图
```



### 分支

``` sh
git branch	# 查看分支
git branch <banch name>	# 创建分支
git branch -d <branch name>	# 删除分支
git checkout <branch name>	# 切换分支
git checkout -b <branch name>	# 创建并切换分支
git merge <branch name>	# 合并 <branch name> 到当前分支
```



