***vscode***
--



### 1. 调试

vscode 调试使用方法也很简单，步骤如下：

- 打开要调试的文件，按f5，编辑器会生成一个launch.json
- 修改launch.json相关内容，主要是name和program字段，改成和你项目对应的
- 点击编辑器左侧长得像蜘蛛的那个按钮
- 点击左上角DEBUG后面的按钮，启动调试
- 打断点，尽情调试（只要你会chrome调试，一模一样）

![](http://i5ting.github.io/vsc/img/2.gif)



### 2. editor 快捷键

#### general

---

**shift+alt+F 格式化代码**



**ctrl+shift+P	打开命令面板    show command palette**    （show all commands）**

**ctrl+`	打开终端    show Terminal**

**ctrl+shift+E	打开资源管理器面板    show explorer**

**ctrl+shift+F	打开全局搜索面板    show search**    

**ctrl+shift+G	打开源代码管理面板    show source control**

**ctrl+shift+D	打开调试面板    show debug**

**ctrl+shift+X	打开扩展面板    show extensions**



**ctrl+shift+M	打开Problems面板**

**ctrl+shift+U	打开Output面板**

**ctrl+shift+Y	打开Debug Console面板**



ctrl+shift+N    打开新Window

ctrl+shift+W    关闭Window

ctrl+w    关闭文件

ctrl+F4    关闭文件



#### basic editing

---

ctrl+d 选中当前单词并搜索（ctrl+c、ctrl+f、ctrl+v）

ctrl+x    剪切整行（vim使用dd剪切整行）

ctrl+shift+K    删除整行

**ctrl+shift+Enter    在上一行插入（vim使用O）**

ctrl+shift+[/]    collapse、uncollapse某一段代码（idea使用ctrl+/-）

ctrl+K S	点击保存全部



#### navigation

---

ctrl+G    到达某一行（vim使用 num+G）

ctrl+P    到达某一个文件（idea使用ctrl+n搜索文件）



#### multi-cursor and selection多光标和选择

---

shft+alt+鼠标选中多行    多行同时编辑



#### display

---

F11    全屏

**alt+space    移动、大小、最大化、最小化、关闭等操作**



#### debug

---

**F5    开始/继续debug**

**shift+F5    stop debug**

F9    切换断点 toggle breakpoint

F11/shift+F11	step into/out

F10    step over



#### integrated terminal集成终端

---

**ctrl+`	打开集成终端**

**ctrl+shift+`	新建集成终端**



**ctrl+shift+p  && 输入shell  && select Default shell（cmd、powershell、git bash）&& ctrl+shift+`**



### 3. cmd 快捷键

```
# open vscode with current directory
code .

# open the current directory in the most recently used code window
code -r .

# create a new window
code -n

# open diff editor
code --diff <file1> <file2>

# see help options
code --help

# disable all extensions
code --disable-extensions .
```



### 4. cool stuff and feature

查看 Help > interactive editor playground



### 5. 提示与技巧(Tips and Tricks)

You'll become familiar with its powerful editing, code intelligence, and source code control features and learn useful keyboard shortcuts.  



1. **Help** > **Interactive Playground**. 

you can interactively try out VS Code's feature. 



4. keyboard reference sheets

Download the keyboard shortcut reference sheet（pdf） for your platform ([macOS](https://go.microsoft.com/fwlink/?linkid=832143), [Windows](https://go.microsoft.com/fwlink/?linkid=832145), [Linux](https://go.microsoft.com/fwlink/?linkid=832144)). 



9. .vscode folder

Workspace specific files are in a `.vscode` folder at the root. For example, `tasks.json` for the Task Runner and `launch.json` for the debugger. 



11. keymaps extension

Are you used to keyboard shortcuts from another editor?

You can install a Keymap extension that brings the keyboard shortcuts from your favorite editor to VS Code. 

Go to **Preferences** > **Keymap Extensions** to see the current list on the [Marketplace](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads). Some of the more popular ones:

- [Vim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)
- [Sublime Text Keymap](https://marketplace.visualstudio.com/items?itemName=ms-vscode.sublime-keybindings)
- [Emacs Keymap](https://marketplace.visualstudio.com/items?itemName=hiro-sun.vscode-emacs)
- [Atom Keymap](https://marketplace.visualstudio.com/items?itemName=ms-vscode.atom-keybindings)



### 6. 链接

[getting started](https://code.visualstudio.com/docs/introvideos/basics)

install and learn the basics of vscode



[customize](https://code.visualstudio.com/docs/introvideos/configure)

personlize vscode through settings, themes, keybindings

[extensions](https://code.visualstudio.com/docs/introvideos/extend)

extend vscode with extensions

[code editing](https://code.visualstudio.com/docs/introvideos/codeediting)

take code editing to the next level

[intellisense](https://code.visualstudio.com/docs/introvideos/intellisense)

receive intelligent code completions

[debugging](https://code.visualstudio.com/docs/introvideos/debugging)

getting started with node,js debugging in vscode

[version control](https://code.visualstudio.com/docs/introvideos/versioncontrol)

learn how to use git version control in vscode