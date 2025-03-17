---
layout: post
title: 'Check the port occupy on Mac OSX'
date: 2019-10-06 09:23:00 +0800
category: from_cnblogs
slug: p20191006092300
---
## Check the port occupy on Mac OSX
`lsof -i :7070`

```text
          COMMAND   PID USER   FD   TYPE    DEVICE SIZE/OFF NODE NAME
          sivoxy    64888   wgg    6u  IPv4 0x6ddd270      0t0  TCP *:gds_db (LISTEN)
```

We have the PID of that app occupying port.

## Locating the executable file of that PID

`ps xuwww -p PID`


PID (64888) is the process id you are looking. for More help on `ps`command you can find with

`man ps`