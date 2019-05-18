---
title: 无法登陆github官网的解决办法
date: 2019-05-09 17:52:09
tags: github
declare: true
toc: true
---
# 前言
今天向博客提交文章的时候，出现了如下的错误，大概的意思就是请确认你是否有权限访问这个仓库，刚开始以为是我的hexo出现了错误，一顿hexo --debug后也没有发现什么异常，后来我就想着查看一下我的github，然后发现无法访问，于是就定位到原因了，无法访问仓库地址。
```java
ssh: connect to host github.com port 22: Connection timed out
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
[41mFATAL[49m Something's wrong. Maybe you can find the solution here: [4mhttp://hexo.io/docs/troubleshooting.html[24m
[33mError: Spawn failed
    at ChildProcess.<anonymous> (D:\work\hexo\node_modules\hexo-util\lib\spawn.js:52:19)
    at ChildProcess.emit (events.js:189:13)
    at ChildProcess.cp.emit (D:\work\hexo\node_modules\cross-spawn\lib\enoent.js:40:29)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:248:12)[39m
```
<!--more-->
# 解决办法
**修改host文件**
```java
Windows 系统位于 C:\Windows\System32\drivers\etc\ 
Android（安卓）系统hosts位于 /etc/ 
Mac（苹果电脑）系统hosts位于 /etc/ 
iPhone（iOS）系统hosts位于 /etc/ 
Linux系统hosts位于 /etc/
绝大多数Unix系统都是在 /etc/

链接：https://www.zhihu.com/question/20732532/answer/253922215

```

1.打开Dns检测|Dns查询 - 站长工具: http://tool.chinaz.com/dns?type=1&host=github.com&ip=

2.在检测输入栏中输入http://github.com官网

3.把检测列表里的TTL值最小的IP输入到hosts里，并对应写上github官网域名
![image](https://note.youdao.com/yws/public/resource/fac2cd9beba4e06cacef59ad04d62009/xmlnote/2A3AD5E8358D498A9CB339A4CEBD0B16/4022)

4.接下来就一个一个试吧，总有一个有用的，目前我现在用的是这个,亲测可用
```java
//把这行代码添加到hosts文件内
192.30.253.112 github.com
```
