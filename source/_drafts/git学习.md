Title:Git 的学习
Date:2015-11-25 17:46
Category:杂物
Tags:Git, 笔记
Slug:Git-learning
Authors:Rainboy


Git学习
================


##资源
[Git教程--廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)

[Git教程--伯乐在线](http://blog.jobbole.com/78960/)

[git-scm](http://git-scm.com/download/)

[git-for-windows](https://git-for-windows.github.io/)



##一.安装

从(http://git-scm.com/download/win) 下载Git


##知识体系

Git
```
| 1    | 2      | 3 | 4 | 5 |
|------+--------+---+---+---|
| 作者 | linus  |   |   |   |
|------+--------+---+---+---|
| 特点 | 分布式 |   |   |   |
|------+--------+---+---+---|


| git init                        | 创建版本库                            |
| git add                         |                                       |
| git commmit -m                  |                                       |
| git status                      |                                       |
| git diff                        |                                       |
| git log                         |                                       |
| git log --pretty=oneline        |                                       |
| git reset --heard HEAD^         |                                       |
| git reset --heard xxxx          |                                       |
| git reflog                      | 记录每一次命令                        |
| git checkout --filename         | 回到最近一次add 或者commit的状态      |
| git reset HEAD filename         | 把暂存区的修改撤销掉，返回add前的状态 |
| rm file,git rm file ,git commit | 删除文件                              |
| rm file,git checkout --file     | 误删                                  |



```
