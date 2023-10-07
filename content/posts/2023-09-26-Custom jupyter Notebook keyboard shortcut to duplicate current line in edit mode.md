---
layout: post
title:  "Custom IPython Notebook keyboard shortcut to duplicate current line in edit mode"
date: 2023-09-26 17:39:32 +0800
categories: python
slug: p20230926173932
---
#  Custom IPython Jupyter Notebook keyboard shortcut to duplicate current line in edit mode



## For Microsoft Windows User (pcDefault)

Create a new JS file under `~/.jupyter/custom/custom.js` if it does not exist, and add the next code:

```js
/**
*
* Duplicate a current line in the Jupyter Notebook
* Used only CodeMirror API - https://codemirror.net
*
**/
CodeMirror.keyMap.pcDefault["Ctrl-Down"] = function(cm){

    // get a position of a current cursor in a current cell
    var current_cursor = cm.doc.getCursor();

    // read a content from a line where is the current cursor
    var line_content = cm.doc.getLine(current_cursor.line);

    // go to the end the current line
    CodeMirror.commands.goLineEnd(cm);

    // make a break for a new line
    CodeMirror.commands.newlineAndIndent(cm);

    // filled a content of the new line content from line above it
    cm.doc.replaceSelection(line_content);

    // restore position cursor on the new line
    cm.doc.setCursor(current_cursor.line + 1, current_cursor.ch);
};
```

**Step 2.**

Restart Jupyter



## For mac user (macDefault)

However if you are a mac user, you need to modify that a bit. You need to change the first line of the code to:

```js
CodeMirror.keyMap.macDefault["Ctrl-Down"] = function(cm){
```

The rest of the code remains the same. Here is the full snippet.

```js
CodeMirror.keyMap.macDefault["Ctrl-Down"] = function(cm){

    // get a position of a current cursor in a current cell
    var current_cursor = cm.doc.getCursor();

    // read a content from a line where is the current cursor
    var line_content = cm.doc.getLine(current_cursor.line);

    // go to the end the current line
    CodeMirror.commands.goLineEnd(cm);

    // make a break for a new line
    CodeMirror.commands.newlineAndIndent(cm);

    // filled a content of the new line content from line above it
    cm.doc.replaceSelection(line_content);

    // restore position cursor on the new line
    cm.doc.setCursor(current_cursor.line + 1, current_cursor.ch);
};
```



## Reference:

 https://stackoverflow.com/questions/29244922/custom-ipython-notebook-keyboard-shortcut-to-duplicate-current-line-in-edit-mode/40505055#40505055