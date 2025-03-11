---
title: svn基础
date: 2024-07-22 12:28:05
tags:

---

# SVN 基础篇及常用指令

## 1、SVN 的一些概念

- **repository（源代码库）:**源代码统一存放的地方
- **Checkout（提取）:**当你手上没有源代码的时候，你需要从 repository checkout 一份
- **Commit（提交）:**当你已经修改了代码，你就需要Commit到repository
- **Update (更新):**当你已经 checkout 了一份源代码， update 一下你就可以和Repository上的源代码同步，你手上的代码就会有最新的变更

## 2、svn常用指令

- svn checkout（path） 从源代码库中拉去代码存放到path路径下

  例：svn checkout svn: // 100.6.6.6:6666/xxx/my
  checkout可以取两个首字母简写为co,后续指令基本全部同理大部分都可以简写

- svn add file 将新加入的文件file加入到版本控制中

  例：svn add xxx.sv (xxx.sv为需要传输的文件)

- svn commit -m "message" --path 提交本地文件到远程库中，添加提交信息message，例如更改了某某功能...  --path表示要提交的文件，一般没有加此项的话会提交当前目录下状态为 m 的文件。

  svn commit -m “xxx” (svn path)
  例：svn commit -m “xxx” xxx.sv

- svn update 将源代码库中的文件同步到本地的文件夹中

  svn update 将版本库更新到本文件夹

  例：svn update (在本地受控文件夹处输入可不加路径,否则需要路径)

- svn status path查看当前目录下的文件状态

  ?：不在svn的控制中；

  M：内容被修改；

  C：发生冲突；

  A：预定加入到版本库

- svn revert -R xxx  将当前路径下的文件强制恢复到某一个版本，xxx表示要恢复到的版本号

  例：svn revert -R 2000 (强制恢复到2000版本)

- svn log path 查看日志

  例：svn log test.php 显示这个文件的所有修改记录，及其版本号的变化

- svn info path 查看文件详细信息

  例如：svn info test.php

- svn help 查看帮助

  例:svn help 某指令名