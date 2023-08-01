---
layout: post
title: 'Storage Location for Unpacked Chrome Extensions'
date: 2019-12-28 22:52:00 +0800
category: from_cnblogs
slug: p20191228225200
---
# Storage Location for Unpacked Extensions

Extension engine does not explicitly change their location or add a reference to its local paths, they are left in the place where there are selected from in all **Operating Systems**.

**Ex:** If i load a unpacked Extension from `E:\Chrome Extension` the unpacked Extension is still in the same location

# Storage Location for Packed Extensions

Navigate to `chrome://version/` and look for **Profile Path**, it is your default directory and Extensions Folder is where all the **`extensions, apps, themes`** are stored


### e.g:

#### Windows

If my **Profile Path** is `%userprofile%\AppData\Local\Google\Chrome\User Data\Default` then my storage directory is:

```
C:\Users\<Your_User_Name>\AppData\Local\Google\Chrome\User Data\Default\Extensions 
```

#### Linux

```
~/.config/google-chrome/Default/Extensions/
```

#### MacOS

```
~/Library/Application\ Support/Google/Chrome/Default/Extensions
```

#### Chromium

```
~/.config/chromium/Default/Extensions
```