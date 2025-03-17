---
layout: post
title: "Zotero 容量满了，用云盘自动同步 - Extending Zotero's syncing folder size by DropBox"
date: 2018-09-30 10:10:00 +0800
category: from_cnblogs
slug: p20180930101000
---
Zotero 容量满了，用云盘自动同步

就同步“storage" 那个文件夹就行了。

https://www.zotero.org/support/sync#alternative_syncing_solutions
https://forums.zotero.org/discussion/29831/what-is-this-pipes-folder-in-my-zotero-folder

What you should do, is
In Zotero re-locate the data directory back out of the Google Drive folder (same instructions as you followed previously). Close Zotero/Firefox.

Disable Google Drive syncing

In Google Drive folder, _move_ (not copy) the "storage" folder out of the Zotero data directory somewhere else inside the Google Drive folder.

Move the Zotero data directory out of Google Drive folder to the folder you selected above in Zotero.

Set up a symbolic link inside the Zotero data directory and point it to the new location of the storage folder. Here's what it _could_ look like. The "storage" folder is at "C:\Google Drive\storage". The Zotero data directory is at "C:\My Data\Zotero data". You would then create a symbolic link inside "C:\My Data\Zotero data\" directory. You would name the link "storage" and you would make it point to "C:\Google Drive\storage"

Open Zotero

Re-enable Google Drive syncing

## Read more:
为Zotero增加论文查看的历史记录功能：[Zotero: add a history feature for paper viewing](https://www.cnblogs.com/sonictl/p/13124264.html)