---
layout: post
title: '你还在想用 nextCloud 自建NAS? 何不试试P2P Sync?'
date: 2020-07-01 14:27:00 +0800
category: from_cnblogs
slug: p20200701142700
---
# 你还在想用 nextCloud 自建NAS? 何不试试P2P Sync?
   
# Are you still using nextcloud as your NAS solutions？ Why not use P2P Sync?

## Why do you need to ditch nextcloud, owncloud, seafile?
   In a word, it's too heavy! Deployment trouble! Synchronization requires too much intranet penetration, simple port mappings often lead to file synchronization errors, and deployments on ARM don't always guarantee Smooth, blah blah blah various holes.

## Peer-to-Peer Sync
   Try **VerySync**, a product based on the P2P Sync design philosophy, which was born to be placed on your router, your computer and your laptop. On Arm, on various desktop OS.
   Based on BT's philosophy of having a net to pass on.
   Claimed to be safe.
   Alternatives: **Resilio Sync** for overseas users.

## 为何你需要弃用 nextcloud?
   一句话，它太重了！布署麻烦！同步对内网穿透的要求太高，简单的端口映射常常导致文件同步错误，ARM上布署不保证每次都顺滑，等等等等各种坑。

## P2P Sync
   试试微力同步这种基于P2P Sync设计理念的产品吧，天生就是拿来让你放在路由器上、Arm上、各种桌面os上的。
   基于BT的理念，有网就能传。
   号称很安全。
   Alternatives: Resilio Sync for overseas users.

## Read More:
 - [What Makes Peer to Peer Syncing Secure and Efficient](https://www.fluxmagazine.com/peer-to-peer-syncing/)
 - [FILE SYNCHRONIZATION SYSTEMS SURVEY](https://arxiv.org/pdf/1611.05346.pdf)
 - [Comparison of file synchronization software](https://en.wikipedia.org/wiki/Comparison_of_file_synchronization_software)
 
-----
===== Quesion asked before I know P2P sync ========

> # 谁能给一个相对稳定完善的NAS解决方案？开源布署在嵌入式板卡计算资源上。

> 20200705: 发现类似resilio sync 这种P2P Sync的软件很是强大啊。nextCloud, seafile 被淘汰的节奏啊。

>   NAS 的基本功能要素：
   - 局域网访问（增删改查），设备支持PC, Mac, 手机，平板，电视等。
   - 基本的文件操作，移动，文件夹，删除，查询，复制，重命名 等。
   - 支持添加下载任务（BT，Magnet, blabla...）
   - 支持多终端app
   - 支持app配置文件自动同步。
   - 多用户共享存储空间，相互隔离。
   - 上述功能支持公网（这就涉及到提供公网访问服务的各种问题）


 >  NAS锦上添花功能：
    - 支持生成url进行资源分享
    - 
    - 待续。。。