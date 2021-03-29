# Useful-Applescript-Snippets
Applescript snippets for daily use of macOS.

分享一些日常使用的 `applescript` 代码片段。

## 切换菜单栏自动隐藏

```applescript
# system preference
tell application "System Preferences"
	activate
	set current pane to pane "com.apple.preference.general"
end tell

# delay
delay 0.3

# check the box
tell application "System Events"
	tell application process "System Preferences"
		tell window "General"
			click checkbox "Automatically hide and show the menu bar"
		end tell
	end tell
end tell

# exit
if application "System Preferences" is running then
	tell application "System Preferences" to quit
end if
```

另一种方法：

```applescript
tell application "System Preferences" to reveal the ¬
	anchor named "main" of ¬
	pane id "com.apple.preference.general"

tell application "System Events" to tell ¬
	process "System Preferences" to tell ¬
	window "General" to tell ¬
	checkbox "Automatically hide and show the menu bar" to ¬
	perform action "AXPress"

quit application "System Preferences"

```

## 切换 Dock 栏自动隐藏

```applescript
tell application "System Events"
	tell dock preferences to set autohide to not autohide
end tell
```

另一种方法：

```applescript
tell application "System Events" to set the autohide of the dock preferences to true -- 打开 Dock 自动隐藏

tell application "System Events" to set the autohide of the dock preferences to false -- 关闭 Dock 自动隐藏
```

设定 Dock 栏的属性（位置、大小等）

```applescript
tell application "System Events"
	tell dock preferences
		if screen edge is left then --如果在左边放到下边 如果在其他地方 放到左边
			set properties to {screen edge:bottom}
		else
			set properties to {screen edge:left}
		end if
	end tell
	set dock size of dock preferences to 0.37
end tell
```



## 设定Finder 窗口格式

```applescript
tell application "Finder"
	tell front window
		set toolbar visible to true --显示工具栏
		set current view to list view --显示为列表格式
		set bounds to {233, 20, 1333, 900} --设定窗口大小
		set sidebar width to 150 --设定侧边栏宽度
	end tell
end tell
```

## 切换暗色模式

```applescript
tell application "System Events" to tell appearance preferences to set dark mode to not dark mode
```

另一种方法：

```applescript
tell application "System Events"
	tell appearance preferences
		if dark mode is false then
			set dark mode to true
		else
			set dark mode to false
		end if
	end tell
end tell
```

## 关闭所有 Finder 窗口（只留下一个）

```applescript
tell application "Finder"
	repeat while window 2 exists
		close window 2
	end repeat
end tell
```

## 获取当前选择文件的路径

即 `/Users/xxx/Desktop/xxx/` 这种格式的地址

```applescript
tell application "Finder"
	set theItems to selection
	set filePath to (POSIX path of (the selection as alias))
end tell
set the clipboard to filePath
```

## 点击菜单栏苹果图标

```applescript
tell application "System Events" to tell (process 1 where frontmost is true)
	click menu bar item 1 of menu bar 1
end tell
```

## 显示 PopClip 菜单

`tell application "PopClip" to appear`