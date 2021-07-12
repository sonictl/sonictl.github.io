---
layout: post
title: 'For quick reference: Configure VSCode for Python'
date: 2021-03-01 11:53:00 +0800
category: from_cnblogs
slug: p20210301115300
---
# For quick reference: Configure VSCode for Python

in command line/Terminal, the environment manager such as conda is correctly installed and configured.

### Create proj. folder

### Run vs code
 - in terminal/cmd, cd to the proj. folder and run `code .`
 - vsCode UI: **File >> Open Folder**

Note: By starting VS Code in a folder, that folder becomes your "workspace". VS Code stores settings that are specific to that workspace in `.vscode/settings.json`, which are separate from user settings that are stored globally.

 to add settings.json in the folder. This will affect only the current folder

- Create a folder named .vscode in the root folder

- Create a file named 

  settings.json

   in that folder and add your settings there.

  ```
  {    
       "settings-key": {
          "setting-1":  true,
          "setting-2":  "../css/",
          "setting-3": "> 0.2%, last 1 version"
      }
  }
  ```

### Select a Python interpreter
opening the Command Palette (⇧⌘P), start typing the Python: Select Interpreter command to search, then select the command. You can also use the Select Python Environment option on the Status Bar if available

Tip: Set the default interpreter: Selecting an interpreter, sets the `python.pythonPath` value in your workspace settings to the path of the interpreter. To see the setting, select **File** > **Preferences** > **Settings** (**Code** > **Preferences** > **Settings** on macOS), then select the **Workspace Settings** tab.

----
### Install python interpreter
link for vsCode+python: https://code.visualstudio.com/docs/python/python-tutorial#_install-a-python-interpreter
link for vsCode+Mac: https://code.visualstudio.com/docs/setup/mac#_launching-from-the-command-line
