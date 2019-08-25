#### Git 最小配置
* 某账号下所有的 Git 仓库都有效
```
git config --global user.name '您的名称' 
git config --global user.email '您的Email'
```

* 只对当前 Git 仓库有效 
```
git config --local user.name '您的名称'
git config --local user.email '您的Email'
```

#### 查看 Git 的配置
* 查看 global 类型的配置项 
```
git config --global --list  
```

* 查看只作用于当前仓库的配置项 
```
git config --local --list  
```

#### 清除 Git 的配置
* 清除 global 类型的配置项   
```
git config --unset  --global 某个配置项 
```

* 清除某个仓库的配置项 
```
git config --unset --local 某个配置项 
```

#### 本地基本操作
* 查看变更情况
```
git status  
```

* 查看当前工作在哪个分支上 
```
git branch -v  
```

* 切换到指定分支 
```
git checkout 指定分支  
```

* 把当前目录及其子目录下所有变更都加入到暂存区
```
git add .  
```

* 把仓库内所有变更都加入到暂存区 
```
git add -A  
```

* 把指定文件添加到暂存区
```
git add 文件1 文件2 文件3  
```

* 创建正式的 commit 
```
git commit  
```

* 比较某文件工作区和暂存区的差异 
```
git diff 某文件
```

* 比较某文件暂存区和 HEAD 的差异
```
git diff --cached 某文件
```

* 比较某文件工作区和 HEAD 的差异
```
git diff HEAD 某文件
```

* 比较工作区和暂存区的所有差异
```
git diff  
```

* 比较暂存区和 HEAD 的所有差异
```
git diff --cached  
```

* 把工作区指定文件恢复成和暂存区一样
```
git checkout 文件1 文件2 文件3  
```

* 把暂存区指定文件恢复成和 HEAD 一样
```
git reset 文件1 文件2 文件3  
```

* 把暂存区和工作区所有文件恢复成和 HEAD 一样
```
git reset --hard  
```

* 用 difftool 比较任意两个 commit 的差异
```
git difftool 提交A 提交B  
```

* 查看哪些文件没被 Git 管控
```
git ls-files --others
```

#### 加塞临时任务的处理
* 把未处理完的变更先保存到 stash 中
```
git stash
```

* 临时任务处理完后继续之前未完的工作
```
git stash pop
```
  > 或者  
```
git stash apply
```
  > pop 不保留 stash，apply 保留 stash

* 查看所有 stash
```
git stash list
```

* 取回某次 stash 的变更
```
git stash pop stash@{数字n}
```

#### 修改个人分支的历史
* 修改最后一次 commit
```
1）在工作区修改文件
2）git add .
3）git commit --amend
```

* 修改中间的 commit (代号X)
```
1）git rebase -i X前面一个commit的id
2）在工作区修改文件
3）git add .
4）git rebase --continue
后续可能需要处理冲突，直到 rebase 结束
```

#### 查看变更的历史
* 当前分支各个 commit 用一行显示
```
git log --oneline
```

* 显示就近的 n 个 commit
```
git log -n
```

* 用图示显示所有分支的历史
```
git log --oneline --graph --all
```

* 查看涉及到某文件变更的所有 commit
```
git log 某文件
```

* 某文件各行最后修改对应的 commit 以及作者
```
git blame 某文件
```
#### 分支与标签
* 基于当前分支创建新分支
```
git branch 新分支
```

* 基于指定分支创建新分支
```
git branch 新分支 已有分支 
```

* 基于某个 commit 创建分支
```
git branch 新分支 某个commit的id
```

* 创建分支并切换到该分支
```
git checkout -b 新分支
```

* 列出本地分支
```
git branch -v
```

* 列出本地和远端分支
```
git branch -av
```

* 列出远端所有分支
```
git branch -rv
```

* 列出名称符合某样式的远端分支
```
git branch -rv -l '某样式'
```

* 安全删除本地某分支
```
git branch -d 拟删除分支
```

* 强行删除本地某分支
```
git branch -D 拟删除分支
```

* 删除已合并到 master 分支的所有本地分支
```
git branch --merged master | grep -v '^\*\|  master' | xargs -n 1 git branch -d
```

* 删除远端 origin 已不存在的所有本地分支
```
git remote prune origin
```

* 给 commit 打上标签
```
git tag 标签名 commid的id
```

#### 两分支之间的集成
* 把A分支合入到当前分支，且为 merge 创建 commit
```
git merge A分支
```

* 把A分支合入到B分支，且为 merge 创建 commit
```
git merge A分支 B分支
```

* 把当前分支基于B分支做 rebase，以便把B分支合入到当前分支
```
git rebase B分支
```

* 把A分支基于B分支做 rebase，以便把B分支合入到A分支
```
git rebase B分支 A分支
```

* 用 mergetool 解决冲突
```
git mergetool
```

#### 和远端的交互
* 列出所有 remote
```
git remote -v
```

* 增加 remote
```
git remote add url地址
```

* 删除 remote
```
git remote remove remote的名称
```

* 改变 remote 的 name
```
git remote rename 旧名称 新名称
```

* 把远端所有分支和标签的变更都拉到本地
```
git fetch remote
```

* 把远端分支的变更拉到本地,且 merge 到本地分支
```
git pull remote名称 分支名
```

* 把本地分支 push 到远端
```
git push remote名称 分支名
```

* 删除远端分支
```
git push remote --delete 远端分支名 
或者
git push remote :远端分支名
```

* 向远端提交指定标签
```
git push remote 标签名
```

* 向远端提交所有标签
```
git push remote --tags
```


