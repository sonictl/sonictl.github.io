---
layout: post
title: 'Zotero: add a history feature for paper viewing  为Zotero添加论文跳转历史记录功能'
date: 2020-06-14 03:52:00 +0800
category: from_cnblogs
slug: p20200614035200
---
During my time with Zotero, I've really enjoyed its various features and the 300MB of file sync space is able to be extended by modifying the path to a synchronized folder under oneDrive or dropBox. reference: [Extending Zotero's syncing folder size by DropBox](https://www.cnblogs.com/sonictl/p/9732977.html)

However, I often get lost when jumping between papers. After jumping from paper A to paper B, I am unable to go back to paper A if I forget its name.

I wish I could write an extension to **add a history feature for paper viewing**. In the official documentation, I didn't find a way to build extension (how to generate a .zoteroplugin from source code?) for Zotero 5.0. And I failed at compiling and installing the the source code for the [hello-world-zotero](https://www.zotero.org/support/dev/sample_plugin) extension, on Zotero 5.0.

Fortunately, with Zotero 5.0's javascript API, I've managed to implement a way of **Manually logs the history of the papers viewed**. It's not quite user friendly yet, but it barely works.

If you know how to do this via extension, drop me a comment, thanks!

-----

我在使用Zotero期间，非常喜欢它的各种功能，并且300MB的文件同步空间也可以通过修改路径扩展到oneDrive或dropBox等文件同步服务器上。

但是，我在论文间跳转时，常常会迷失方向。从A论文跳转到B论文后，无法回退至A论文。

我希望可以写一个扩展程序来增加论文查看的历史记录功能。由于我并没有在官方文档中找到Zotero5.0的扩展程序build方法，我失败于将[hello-world-zotero](https://www.zotero.org/support/dev/sample_plugin)扩展的源码进行编译并安装进Zotero5.0.

还好，我通过 Zotero 5.0 的 javascript API, 勉强实现了一个手动的记录所浏览的paper的历史记录。它还不太友好，但勉强能用。

如果你知道如何通过扩展来实现，请给我留言，谢谢！

### 使用环境：Test environment:
 `Zotero v5.0.87`
 `tested on Mac OSX 10.15.4` 
  windows user can try the same way and comment on how it works. thx!

### 使用方法：Useage:
`Zotero` -> `Tools` -> `Develop` -> `Run javascript`
    
 step1. 先执行第1段代码，进行变量的初始化：Initialize the variable by executing the first patch of code:
```javascript
//=== initialize the history ====
var items = Zotero.getActiveZoteroPane().getSelectedItems();
var cur_itemKey = items[0].key;
var tmp_key = cur_itemKey;
var key_paper_list = 'List of keys of papers your selected:\n';
cur_itemKey += '\n';
```

 step2. 然后将第1段代码替换成第2段代码： Then replace the first patch of code with the below.

```javascript
//==== run above once, and modify above 5 lines into: ====
items = Zotero.getActiveZoteroPane().getSelectedItems();
cur_itemKey = items[0].key + '\n';
if(items[0].key !=tmp_key){
	tmp_key = items[0].key;
	key_paper_list += cur_itemKey;
}
else{
    key_paper_list += '';
}
```

 step3. 回到Zotero主界面，进行论文浏览，每切换一次论文，就回到Run javascript界面执行一次第2段代码，进行历史记录。Return to the Zotero main screen and browse papers, returning to Run every time you switch papers. The javascript interface executes the 2nd code once to make a history.
    
 step4. 通过搜索，直接输入key即可回溯浏览记录。go back through the browsing history by searching and entering the key directly.

## Tips:
   on Mac OSX, use `Command`+` to switch between javascript running interface and Zotero paper viewing interface.



## 参考：Reference:
[Zotero JavaScript API](https://www.zotero.org/support/dev/client_coding/javascript_api#get_information_about_an_item)
[Zotero Source Code](https://www.zotero.org/support/dev/source_code)
