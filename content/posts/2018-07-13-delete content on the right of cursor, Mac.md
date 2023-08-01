---
layout: post
title: 'delete content on the right of cursor, Mac'
date: 2018-07-13 04:20:00 +0800
category: from_cnblogs
slug: p20180713042000
---
## Forward delete content on the right of cursor, Mac

It's not convenient to press `Fn`+`delete` to delete content on the right of cursor, on Macintosh. They're too faraway. 

#### Forward Deleting

For instance: 
  For test: `ab|cd`, the cursor is between `b` and `c`,  **deleteForward** makes the text  `ab|cd` into `ab|d` .

### Use Mastero Keyboard tool:

**Adding the Macro for DeleteForward**

<img src="/assets/images/image-201807130420001001.png" alt="image-20220519175936884" style="zoom:50%;" />



**For adding the action, select 'KeyStroke':** 
<img src="/assets/images/image-201807130420001002.png" alt="image-20220519175936884" style="zoom:50%;" />


#### Another method using Karabiner tool (not tested)

Use Karabiner to remap `RightShift`+`delete` to `Fn`+`delete` so that we can only one right hand to implement that deleting backward.

Edit the json ~/.config/karabiner/karabiner.json

ReInstall this app to make it take effect. Sounds strange but i didn't find approprate way to take it effect.