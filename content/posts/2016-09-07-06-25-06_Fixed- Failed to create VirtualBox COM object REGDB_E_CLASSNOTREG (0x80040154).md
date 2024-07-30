---
layout: post
title:  "Fixed- Failed to create VirtualBox COM object REGDB_E_CLASSNOTREG (0x80040154)"
date: 2016-09-07 06:25:06 +0800
tags: 
slug: p20160907062506
---

# Fixed- Failed to create VirtualBox COM object REGDB_E_CLASSNOTREG (0x80040154)





## Fix: Failed to create VirtualBox COM object REGDB\_E\_CLASSNOTREG (0x80040154)



[January 16, 2014](http://www.kirtrainford.co.uk/fix-failed-create-virtualbox-com-object-regdb_e_classnotreg-0x80040154/ "Permalink to Fix: Failed to create VirtualBox COM object REGDB_E_CLASSNOTREG (0x80040154)")
  [Blog](http://www.kirtrainford.co.uk/category/random-posts/)
  [0x80040154](http://www.kirtrainford.co.uk/tag/0x80040154/),  [com object](http://www.kirtrainford.co.uk/tag/com-object/),  [failed to create](http://www.kirtrainford.co.uk/tag/failed-to-create/),  [virtualbox](http://www.kirtrainford.co.uk/tag/virtualbox/)
  [Kirt](http://www.kirtrainford.co.uk/author/admin/ "View all posts by Kirt")

When opening Oracle VM one day, you stumble upon this message (note the error code 0x80040154):



> Failed to create the VirtualBox COM object.  The application will now terminate. Callee RC: REGDB\_E\_CLASSNOTREG (0x80040154)
> 
> 
> virtualbox创建COM对象失败，程序终止。召唤者 RC: REGDB\_E\_CLASSNOTREG (0x80040154) 。 【解决】


The Virtual Machine service isn’t running. A reinstall would probably work but there is an simpler fix:


Run Command Line or “cmd”


Type the following into the command line and hit enter (including the quotes):



> chdir “C:\Program Files\Oracle\VirtualBox”


Then:



> VBoxSVC /ReRegServer


And:



> regsvr32 VBoxC.dll


 


You should see a popup window notifying you of your success. Try opening VM now, it should work.




