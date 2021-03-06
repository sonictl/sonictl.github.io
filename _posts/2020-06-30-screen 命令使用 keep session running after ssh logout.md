---
layout: post
title: 'screen 命令使用 keep session running after ssh logout'
date: 2020-06-30 06:44:00 +0800
category: from_cnblogs
slug: p20200630064400
---
# screen 命令使用
ref: https://handerfly.github.io/linux/2019/03/31/Screan%E5%91%BD%E4%BB%A4%E7%9A%84%E4%BD%BF%E7%94%A8/

## 彻底搞清楚screen 命令使用

*Posted by BenderFly on March 31, 2019*

> screen 是一个非常有用的命令，提供从单个 SSH 会话中使用多个 shell 窗口（会话）的能力。当会话被分离或网络中断时，screen 会话中启动的进程仍将运行，你可以随时重新连接到 screen 会话. screen is a very useful command that provides the ability to use multiple shells from a single SSH session. The ability to window (session). When a session is separated or the network is interrupted, the processes started in the screen session will still be running, and you can always reconnect to the screen Sessions

# 查看是否安装

```
screen -v
Screen version 4.00.03 (FAU)
```

# 安装Screen

**CentOS/RedHat/Fedora**

```
yum -y install screen
```

**Ubuntu/Debian**

```
apt-get -y install screen
```

# 参数

**参数说明**

```
-A 　将所有的视窗都调整为目前终端机的大小。
-d     <作业名称> 　将指定的screen作业离线。
-h     <行数> 　指定视窗的缓冲区行数。
-m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
-r      <作业名称> 　恢复离线的screen作业。
-R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
-s 　指定建立新视窗时，所要执行的shell。
-S    <作业名称> 　指定screen作业的名称。
-v 　显示版本信息。
-x 　恢复之前离线的screen作业。
-ls或--list 　显示目前所有的screen作业。
-wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。
```

**常用screen参数**

```
screen -S session_name           # 新建一个叫session_name的session
screen -ls（或者screen -list）   # 列出当前所有的session
screen -r session_name            # 回到session_name这个session
screen -d session_name           # 远程detach某个session
screen -d -r session_name        # 结束当前session并回到session_name这个session
```

**在每个screen session 下，命令以 ctrl+a、ctrl-a开头，常用的几个操作如下：**

```
ctrl-a x   # 锁住当前的shell window，需用用户密码解锁
ctrl-a d   # detach，暂时离开当前session，将当前 screen session 转到后台执行，并会返回没进 screen 时的状态，此时在 screen session 里，每个shell client内运行的 process (无论是前台/后台)都在继续执行，即使 logout 也不影响
ctrl-a z   # 把当前session放到后台执行，用 shell 的 fg 命令则可回去。
ctrl-d     # 中止当前所在的screen会话，并退出
```

**其他说明**

如果在执行 screen -r pts-0.localhost提示下面信息，无法跳到pts-0.localhost这个会话![screen](https://raw.githubusercontent.com/handerfly/handerfly.github.io/master/img/screen-error.png)
此时请先执行 screen -d pts-0.localhost,再执行screen -r pts-0.localhost，即可跳到pts-0.localhost这个会话（即先detach，再连接）

# 启动一个 screen 会话

命令行中输入 screen 来启动它

```
screen
#要使用会话名称创建新会话，请运行以下命令：
screen -S name
```

# 从 screen 会话中分离

要从当前的 screen 会话中分离，你可以按下 Ctrl-A 和d。所有的 screen 会话仍将是活跃的，你之后可以随时重新连接。

# 重新连接到 screen 会话

如果你从一个会话分离，或者由于某些原因你的连接被中断了，你可以使用下面的命令重新连接：

```
screen -r
```

如果你有多个 screen 会话，你可以用 ls 参数列出它们。

```
screen -ls

There are screens on:
7880.session    (Detached)
7934.session2   (Detached)
7907.session1   (Detached)
3 Sockets in /var/run/screen/S-root.
```

在我们的例子中，我们有三个活跃的 screen 会话。因此，如果你想要还原 “session2” 会话，你可以执行：

```
screen -r 7934
```

或者使用 screen 名称。

```
screen -r -S session2
```

# 中止 screen 会话

有几种方法来中止 screen 会话：

- 你可以按下 Ctrl+d ，或者

- 在命令行中使用 exit 命令。

### 要查看screen命令所有有用的功能，可以查看 screen 的 man 手册：

```
man screen

NAME
screen - screen manager with VT100/ANSI terminal emulation

SYNOPSIS
screen [ -options ] [ cmd [ args ] ]
screen -r [[pid.]tty[.host]]
screen -r sessionowner/[[pid.]tty[.host]]
```

打完收工