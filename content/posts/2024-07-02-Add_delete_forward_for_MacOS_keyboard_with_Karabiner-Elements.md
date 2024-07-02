---
layout: post
title:  "Add delete_forward for MacOS keyboard with Karabiner-Elements"
date: 2024-07-02 10:58:33 +0800
tags: 
slug: p20240702105833
---


# Add a shortcut for 'delete_forward' on Mac OS

Windows OS keyboard has 'backspace'(deleteBackward_or_delLeftward) and 'delete'(deleteForward_or_delRightward). However, MacOS has not deleteForward.

Even though we have `fn`+`delete` to achieve this, this occupies your TWO hands. Unconvenient. 

Here, we use the `Karabiner-Elements` app, which is an open-source free app, to patch this issue.

ps: in MacOS, the keyCode 117 means deleteForward_or_delRightward

======= Steps =======

1. Install and launch 'Karabiner-Elements', which is an open-source free app.
2. Add a `complex modifications` as below （I was mapping **option+delete** into **delete_forward**）:

```

{
    "description": "Option+Delete to keycode 117",
    "manipulators": [
        {
            "from": {
                "key_code": "delete_or_backspace",
                "modifiers": {
                    "mandatory": [
                        "option"
                    ]
                }
            },
            "to": [
                {
                    "key_code": "delete_forward",
                    "modifiers": []
                }
            ],
            "type": "basic"
        }
    ]
}

```



A test about inserting img into my blog:

![img](/img/20240702.png)

```
This is referred by:
    ![img](/img/20240702.png)
```

-----------

![img](https://sonictl.github.io/img/20240702.png)

```
This is referred by:
    ![img](https://sonictl.github.io/img/20240702.png)
```

