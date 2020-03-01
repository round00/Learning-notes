# 记录一些git的命令

- git branch <branch\>				创建分支
- git checkout <branch\>			切换分支
- git checkout <commit_hash\>		分离HEAD，使其指向提交记录而不是分支。由于hash值很长，可以									只输入前几个字符就可以了
- git checkout -b <branch\>			切换分支，如果分支不存在则创建
- git merge <branch\>				将<branch\>合并到当前分支上
- git rebase <branch\>				将当前分支的内容移动到<branch\>上，与merge不同
- git checkout <branch\>^			分离HEAD，使其指向<branch>的上一个分支
- git checout HEAD^					分离HEAD,使其指向HEAD的上一个分支
- git branch -f <branch\> <reference\>强制将<branch\>指向<reference\>指向的位置

### git checkout <something> 实际上是将HEAD指向something的位置，这个位置可以是一个<branch>，也可以是一个提交记录，还可以是一个相对引用(^和~)的位置
- git reset HEAD^					撤销一个提交记录，将提交回退到HEAD的前一个。这个**对远程仓库无效**
- git revert <branch\>				将<branch\>回退一个版本。回退的方法是新建一个提交，使得该								提交的内容和当前提交的上一个提交一样，这样就可以修改远程仓库了
- git cherry-pick C1 [C2 C3...]		将提交C1 C2 C3等移动到当前分支上，
- git rebase -i HEAD~4				将HEAD的前4个提交通过交互式界面进行调整。(不过命令行客户端好像不太好用)
- git tag v1 C1						给C1打上v1标签，如果不指定C1，那么将给当前HEAD所在提交打标签
- git describe <C>					从提交C向上回溯找出一个打标签的节点
