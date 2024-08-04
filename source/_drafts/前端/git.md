# GIT



### 比较差异

``` sh
git diff # 比较工作区与暂存区的差异
git diff --cached # 比较暂存区与版本区的差异
git diff master # 比较工作区与版本区的差异
```

## 查看账户

``` sh

```

### 基本

``` sh
git init # 初始化
git status # 查看当前仓库状态信息
git status -sb
git add <file> # 将文件加入暂存区
git revert
git revert HEAD # 撤销上一个提交
git revert [倒数第一个提交] [倒数第二个提交] # 抵消多个提交
git reset HEAD <file> # 暂存区与版本区保持一致
git reset --hard <version> # 恢复版本区指定版本的内容到工作区
git checkout <file>	# 暂存区（或者版本区）覆盖工作区的内容
git rm <file> --cached # 删除暂存区文件
git commit -a -m '' # git add . git commit -m ''
git commit . --amend # 将当前 commit 合并到上次 commit
git refolg # 查看引用版本号
git log	# 查看日志
git status -sb	# 显示所有的状态和总结
git rebase -i HEAD~3 # 合并最近三次 commit
git rm --cached <file>	# 将 file 从 git 仓库删除
git checkout -- [filename] # 找回本次修改之前的文件
git stash
git stash pop

git log --oneline -- graph # 查看历史操作图
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit -- | less
```



### 分支

``` sh
git branch	# 查看分支
git branch <banch name>	# 创建分支
git branch -d <branch name>	# 删除分支
git checkout <branch name>	# 切换分支
git switch <branch name>	# 切换分支
git checkout -b <branch name>	# 创建并切换分支
git merge <branch name>	# 合并 <branch name> 到当前分支
```



``` sh
git remote add origin git@github.com:example.git	# 将本地仓库与远程仓库关联
git remote set-url origin git@github.com:example.git	# 重新设置远程仓库
```



## Rebase



``` sh
git rebase -i HEAD~2 # HEAD~ 后的数字表示选择最新的几次提交，数字 2 选择最近两次提交
```

然后会出现编辑器有如下内容

``` sh
pick b52760a commit one
pick bd260cc commit two
```

将需要合并提交前面的 pick 改为 s(fixup) 或者 f(squash)

``` sh
pick b52760a commit one # 保留的提交
s bd260cc commit two # 被合并的提交
```

s 和 f 都可以将提交合并到 pick 中；s 会保留提交信息，而 f 则不保留提交信息



## 常用功能

- 修改提交信息

  `git commit --amend`
