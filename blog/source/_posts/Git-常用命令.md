---
title: Git 常用命令
date: 2017-9-16 15:33:11
tags: Git
categories: Others
---

## Git 将服务器代码更新到本地

1. git status（查看本地分支文件信息，确保更新时不产生冲突）
2. git checkout -- [file name]（若文件有修改，可以还原到最初状态；若文件需要更新到服务器上，应该先merge到服务器，在更新到本地）
3. git branch（查看当前分支状况）
4. git checkout [remote branch]（若分支为本地分支，则需要切换到服务器的远程分支）
5. git pull（若命令执行成功，则更新代码成功）

<!-- more -->
 
## Git push常用方法

1. git push origin master（如果远程分支被省略，则表示将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），如果该远程分支不存在，则会被新建）
2. git push origin : refs/for/master（如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支，等同于 git push origin --delete master）
3. git push origin（如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支）
4. git push（如果当前分支只有一个远程分支，那么主机名都可以省略，形如git push，可以使用git branch -r 查看远程的分支名）
 
## Git 代码冲突解决方案

如果系统中有一些配置文件在服务器上做了配置修改，然后后续开发中新添加一些配置项的时候，在发布这个配置文件的时候，会发生代码冲突：error: Your local changes to the following files would be overwrtten by merge:protected/config/main.php.    Please,commit your changes or stash them before you can merge.

    如果希望保留生产服务器上所作的改动，仅仅并入新配置项，可以这样处理：
```
git stash
git pull
git stash pop
```
	然后可以使用 Git diff -w + 文件名  来确认代码自动合并的情况

    如果希望代码库中的文件完全覆盖本地工作版本，方法如下：
```
git reset --hard
git pull
```
    其中 git reset是针对版本。

## Git Commit 与 Git push的区别
```
git commit 操作的是本地库， git push操作的是远程库。
git commit 是将本地修改过的文件提交到本地库中。
git push 是将本地库中的最新信息发送给远程库。
```
 

## git add -A 和 git add . 的区别
```
git add . : 它会监控工作区的状态树，使用 git add . 会把工作时的所有变化提交到暂存区，包含文件修改内容以及新文件，但不包括被删除的文件。
git add -u: 它仅加农被 add 的文件，会将被修改的文件和被删除的文件提交到暂存区。不会提交新文件。
git add -A : 提交所有变化
```