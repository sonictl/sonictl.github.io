---
layout: post
title:  "Hammerspoon 备忘录｜用 macOS 自动化工具干了什么 & 安装、权限、配置、救急全攻略"
date: 2026-11-22 14:22:00 +0800
categories: Tech
slug: p20261122142200
---

> 一句话版：Hammerspoon（HS）是 macOS 上基于 Lua 的自动化工具——绑定全局快捷键、窗口管理、事件监控、脚本调度，全都能干。这篇是我自用存档：先说我在 Mac 上用 HS 实际干了什么，再说怎么装、怎么配、踩了哪些坑、被锁死怎么自救，实例放最后。

## 一、先说我在 Mac 上用 HS 干了什么

HS 的定位一句话形容：**给 macOS 加的“系统级外挂”**。所有功能都靠 `~/.hammerspoon/init.lua` 里的一段 Lua 代码驱动，不挑应用、全局生效。我自己长期在用的几个场景：

- **窗口管理**：把窗口甩到屏幕左/右/上/下半屏、快速分屏，不用鼠标拖来拖去；
- **全局快捷键**：任何一个 App 里都能触发的热键，比如一键打开指定 App、一键跳到某目录；
- **自动切桌面/切空间**：鼠标滚轮左右倾斜直接切换桌面，比如，罗技 M720 滚轮左右倾斜切换桌面，不用去触控板手势；
- **定时任务**：写个 `hs.timer` 定时提醒、定时执行脚本；
- **事件监听**：`hs.eventtap` 监听鼠标/键盘事件，按需触发动作；热键、改键用。
- **状态显示**：菜单栏显示电池、网络等自定义信息。

> 核心思想：**把重复操作变成一行热键/一段脚本**。下面这份备忘录就是围绕这些用途的完整配置与排障手册。

## 二、前置说明

- 最低系统：macOS ≥ Ventura（13）
- 配置入口：`~/.hammerspoon/init.lua`，这是主配置文件，所有 Lua 脚本写在这里
- `~`：代表当前用户家目录
- 核心痛点：**辅助功能权限是一切功能的前提；隐藏菜单栏图标之后，很容易把自己锁死**

## 三、彻底卸载（重装前清理，踩坑必做）

如果之前装过、图标出不来、权限错乱，先清理干净再重装。

1. 退出 Hammerspoon：菜单栏图标 → Quit Hammerspoon
2. 把 `/Applications/Hammerspoon.app` 拖到废纸篓
3. 清理系统权限（非常关键）
   - 系统设置 → 隐私与安全性 → 辅助功能
   - 在列表找到 Hammerspoon，选中，点 `-` 删除
4. 如果之前装过 `hs` 命令行工具

```bash
 rm -f /usr/local/bin/hs
 rm -f /usr/local/share/man/man1/hs.1
```

5. ✅ **保留配置文件夹**：`~/.hammerspoon` 不动，里面的 `init.lua`、Spoon 插件可以复用，不用删

## 四、安装（二选一，优先 Homebrew）

### 方式 1：Homebrew（推荐，后续升级简单）

```bash
 brew install --cask hammerspoon
```

安装完成，启动台打开 Hammerspoon。

### 方式 2：官网手动下载

官网地址：https://www.hammerspoon.org/ 下载 dmg，将 Hammerspoon.app 拖进「应用程序」。

> 如果提示“无法打开，来自未识别的开发者”：系统设置 → 隐私与安全性，下滑点「仍要打开」

## 五、首次启动 & 权限（最容易翻车的一步）

打开 Hammerspoon 之后，会弹窗请求【辅助功能】权限：

- 点击「打开系统设置」，勾选 Hammerspoon
- **没有这个权限：菜单栏图标、快捷键、窗口管理、鼠标事件监听全部失效**
- 勾选后，右上角状态栏才会出现绿色小图标

> 开机自启：菜单栏 Hammerspoon → 勾选 `Launch Hammerspoon at login`

## 六、配置目录与文件

打开配置目录（终端执行）

```bash
 mkdir -p ~/.hammerspoon
 open ~/.hammerspoon
```

会直接弹出访达窗口。

主配置文件：`init.lua`

- 新建文本文件，粘贴 Lua 代码，保存命名为 `init.lua`
- ❗坑：mac 默认自动加上 `.txt`，变成 `init.lua.txt`，会识别失败。访达右键「显示简介」，删掉多余后缀。

编辑配置的几种方式：

1. `nano ~/.hammerspoon/init.lua`
2. VS Code / Sublime / Neovim 直接打开文件
3. Hammerspoon 菜单：`Open Config`

## 七、重载配置 & 控制台

修改完脚本之后，必须重载才能生效。三种重载方式：

1. 菜单栏图标 → `Reload Config`（最简单）

2. 如果安装了 IPC：终端

```bash
 hs -c "hs.reload()"
```

3. 自己绑定快捷键重载

控制台（排查报错神器）

- 菜单：`Console`
- 红色输出 = 语法错误、API 调用错误；写日志：`hs.printf("日志内容")`

安装 IPC（启用终端 `hs` 命令） 在 Hammerspoon 的 Console 里面执行这一行：

```lua
 hs.ipc.cliInstall()
```

之后就可以在终端调用 Hammerspoon 的 Lua 代码。

## 八、重点：隐藏菜单栏图标之后，如何自救

> 我自己的需求：桌面干净，不想常驻菜单栏图标；但一旦关掉，没有预案就会被困住。

### 救急快捷键（一定要写进你的 init.lua，先重载，再隐藏图标）

```lua
-- 加载 IPC
require("hs.ipc")

-- Cmd+Alt+Shift + , 打开设置面板
hs.hotkey.bind({"cmd","alt","shift"}, ",", function()
    hs.openPreferences()
end)

-- Cmd+Alt+Shift + C 打开控制台
hs.hotkey.bind({"cmd","alt","shift"}, "c", function()
    hs.openConsole()
end)

-- Cmd+Alt+Shift + M：临时恢复菜单栏图标（救命键）
hs.hotkey.bind({"cmd","alt","shift"}, "m", function()
    hs.menuIcon(true)
end)

-- 重载配置快捷键
hs.hotkey.bind({"cmd","alt","shift"}, "r", function()
    hs.reload()
    hs.alert.show("Hammerspoon 配置已重载")
end)

-- 关闭菜单栏图标（放在最后，前面快捷键生效之后再执行）
hs.menuIcon(false)

-- 可选：关闭 Dock 图标，风险更高，谨慎开启
-- hs.dockIcon(false)
```

### 如果已经把自己锁死（无菜单、无快捷键、无 IPC）

最坏情况：同时关闭 menuIcon、dockIcon，并且没有绑定任何救急快捷键。 方案：杀掉进程重启，重启瞬间会短暂显示菜单栏图标，立刻点进去改设置。

```bash
pkill Hammerspoon && open -a Hammerspoon
```

其他打开设置的方式（进程在后台运行时）

1. Spotlight：`Cmd + 空格`，输入 Hammerspoon，激活程序，然后 `Cmd + ,` 打开偏好设置

2. 有 IPC：

```bash
 hs -c "hs.openPreferences()"
 hs -c "hs.menuIcon(true)"
```

## 九、基础测试脚本（用来验证环境是否正常）

替换 `init.lua`，重载，按下 `Cmd + Ctrl + T`，弹出提示代表环境正常。

```lua
hs.hotkey.bind({"cmd","ctrl"}, "t", function()
    hs.alert.show("✅ Hammerspoon 运行正常")
end)
```

## 十、常用工作流

1. 打开 `init.lua`，写 Lua 自动化逻辑（窗口布局、快捷键、应用切换、定时任务）
2. 保存文件
3. 按救急快捷键 `Cmd+Alt+Shift+R` 重载
4. 如果失效，打开控制台看红色报错，修复语法
5. 稳定后，保持菜单栏图标关闭，靠快捷键使用

## 十一、高频踩坑清单（备查）

1. **辅助功能权限丢失**：mac 更新系统后经常重置权限，鼠标监听、热键会直接失效，优先检查这里
2. **文件名后缀错误**：`init.lua.txt`， Hammerspoon 不认
3. **刘海屏 Mac**：菜单栏空间不足，图标被挤掉，不是程序问题
4. **代码语法报错**：会导致整个脚本部分/全部失效，去 Console 看行号
5. **顺序问题**：先绑定救急快捷键，再执行 `hs.menuIcon(false)`；顺序颠倒，快捷键不会加载
6. 更新 macOS、更新 Hammerspoon 之后，建议重新确认辅助功能权限
7. `hs.eventtap` 鼠标/键盘监听类脚本，对辅助功能权限要求严格，权限一丢直接静默失效

## 十二、扩展：Spoon

Spoon 是 Hammerspoon 的组件库，放到 `~/.hammerspoon/Spoons/`，然后在配置里 `require`，适合引入现成窗口管理、wifi、电池脚本。

## 十三、极简自用模板（可以直接长期使用）

```lua
require("hs.ipc")

-- 救急三件套 + 重载
hs.hotkey.bind({"cmd","alt","shift"}, ",", function() hs.openPreferences() end)
hs.hotkey.bind({"cmd","alt","shift"}, "c", function() hs.openConsole() end)
hs.hotkey.bind({"cmd","alt","shift"}, "m", function() hs.menuIcon(true) end)
hs.hotkey.bind({"cmd","alt","shift"}, "r", function()
    hs.reload()
    hs.alert.show("配置重载完成")
end)

-- 你的业务脚本写在这里
-- hs.hotkey.bind(...)

-- 隐藏菜单栏图标
hs.menuIcon(false)
```

## 十四、实战案例：罗技 M720 滚轮左右倾斜切换桌面

> M720 的滚轮支持左右倾斜。mac 默认把倾斜识别成水平滚动，下面脚本捕获物理倾斜事件，映射为「切换空间」。 原理：区分「连续平滑滚动（触控板/鼠标平滑）」和「非连续物理滚轮倾斜」，加节流防止多次触发。 当前映射：滚轮向左倾斜 → 上一个桌面；滚轮向右倾斜 → 下一个桌面，使用 `Ctrl + Fn + ← / →`。

```lua
local lastFire = 0

-- 监听鼠标滚轮事件，实现 M720 滚轮左右倾斜切桌面
tilt = hs.eventtap.new({ hs.eventtap.event.types.scrollWheel }, function(e)
    local P = hs.eventtap.event.properties
    
    -- 过滤触控板或鼠标平滑连续滚动，只保留物理滚轮左右倾斜
    if e:getProperty(P.scrollWheelEventIsContinuous) ~= 0 then 
        return false 
    end
    
    -- 获取水平滚动增量 (Axis2 即 X 轴)
    local dx = e:getProperty(P.scrollWheelEventDeltaAxis2)
    if dx == 0 then 
        return false 
    end
    
    -- 节流防抖（0.35秒内只响应一次，防止连续滚动切换过快）
    local now = hs.timer.secondsSinceEpoch()
    if now - lastFire > 0.35 then
        lastFire = now
        
        -- dx > 0 表示向左倾斜，触发 Ctrl + Fn + ← 切换桌面
        -- dx < 0 表示向右倾斜，触发 Ctrl + Fn + → 切换桌面
        local key = dx > 0 and "left" or "right"
        hs.eventtap.keyStroke({ "ctrl", "fn" }, key, 0)
    end
    
    -- 返回 true 截断该事件，防止系统默认的水平滚动行为
    return true
end)

tilt:start()
```

### M720 脚本额外注意事项

1. **必须开启辅助功能权限**，否则 `hs.eventtap` 收不到鼠标事件；
2. 如果同时装了 Logi Options / Options+，建议把滚轮倾斜功能在罗技软件里关闭，避免冲突；
3. `0.35` 是节流间隔，可以按需调小（更灵敏）或者调大（更稳重）；
4. `return true` 代表吃掉原始水平滚动事件；如果你想保留原有滚动，改成 `return false`；
5. 如果方向反了：交换 `left` / `right`，或者改成 `dx < 0 and "left" or "right"`。
