---
title: git基础
date: 2025-03-11 14:28:00
tags:
---


---
title: git基础
date: 2024-07-22 14:02:48
tags:

---

# Git基础篇及一些常用指令

## 1、Git的一些基础

- 分布式：每个人的计算机都有一个完整的代码仓库，包括所有的版本历史。使得开发者可以在离线的情况下进行工作。
- 仓库(Repository)：存储项目文件和版本历史的地方，可以是本地仓库或者远程仓库。
- 工作区(Working Directory)：当前正在编辑的文件所在的目录。
- 暂存区(Staging Area)：用于暂时保存提交文件的区域。
- 提交(Commit)：将暂存区的文件提交到本地仓库的操作，每次提交都一个唯一的标识，哈希。
- 分支(Branch):用于并行开发的独立线，主分支通常是main 或 master。
- 远程仓库(Remoe Repository)：托管在服务器上的仓库，语序团队成员之间共享代码。

## 2、Git常用指令

- git config  --global user.name 用户名

  添加仓库用户名

- git config --global user.email 邮箱

  设置用户邮箱

- git init

  初始化仓库

- git add .

  添加文件到暂存区

- git commit -m "自定义名称"

  将暂存区内容添加到仓库中

- git pull

  下载远程代码并合并

- git push

  上传远程代码并合并

- git push origion "项目分支名称"

  上传远程代码并合并。

- git remote -v

  查看当前所有远程地址别名

- git remote add 别名 远程地址

  创建远程仓库别名

- git clone 远程地址

  克隆远程仓库到本地，（不需要登录账号）

- git branch <分支名称>

  创建分支

- git branch -v

  查看分支

- git merge 分支名称 / git merge dev

  合并分支

- git check out "项目分支名称呼"

  切换分支，也可以使用这个指令新建分支然后直接切换到新的分支上

- git log

  查看历史提交记录

- git blame  <file>

  以列表形式查看指定文件的历史修改记录

- git reflog

  查看历史记录

- git reset --hard 版本号

  版本穿梭



### 强制恢复之前的提交，(适用于个人项目，不影响他人）

1、查找上一个正确的提交

```
git log --oneline
```

2、回滚到正确的提交

```
git reset --hard 哈希
```

3、强制推送到远程仓库

```
git push origin main --force
```

注意: 这会覆盖远程仓库的提交历史，慎用！