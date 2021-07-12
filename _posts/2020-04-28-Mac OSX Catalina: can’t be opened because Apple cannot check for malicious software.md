---
layout: post
title: 'Mac OSX Catalina: can’t be opened because Apple cannot check for malicious software'
date: 2020-04-28 07:59:00 +0800
category: from_cnblogs
slug: p20200428075900
---

Mac OSX Catalina: can’t be opened because Apple cannot check for malicious software
macOS cannot verify that this app is free from malware.

Solution:
 - check with Security&Privacy settings under System Preferences to lift the Gatekeeper blocks, and fix alert messages.
 - when you are using Catalina, the warning message reappears every time you open the non-notarized app
 - open Terminal and run the cmd below

```
xattr -d com.apple.quarantine /Applications/<your App's Name>.app

e.g.: 
xattr -d com.apple.quarantine /Applications/qbittorrent.app
```