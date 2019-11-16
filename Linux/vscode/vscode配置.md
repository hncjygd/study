[toc]
# vscode基础

## vscode快捷键

- ctrl+shift+p 打开命令窗口
- ctrl+p 打开文件，可以打开左侧文件价中的文件
- ctrl+w 关闭文件
- ctrl+s 保存文件
- ctrl+z 撤销，如果使用vim插件也可以使用
- ctrl+shift+i 格式化文本样式(统一缩进，格式化表格等等)
- alt+1/2/3 切换文件
- ctrl+` 打开关闭终端
- 其他快捷键使用vim来进行吧

## 插件推荐

### vim
基本上模拟了所有vim的功能。普通模式 插入模式 视图模式等都可以有。q键盘宏也可以正确录入。:number :relativenumber等命令也可以有。多窗口操作也有。多标签也可以有。

### vscode-icons
为vscode的资源管理器中的文件夹以及文件添加图标

### ProjectManager
在vscode中，是通过文件夹来组织项目文件的，所以要想打开一个项目，就必须每次都通过文件-打开文件夹命令来实现。如果项目非常多而且文件夹所放置的位置也不同的话会非常的影响工作。此时可以使用该插件将文件夹保存为一个工程，每次通过命令窗口打开工程就可以了。  

- Save Project 将打开的文件夹保存为一个项目
- List Project to Open / shift+alt+p 打开一个项目(其实就是直接将项目所代表的文件夹打开)

### Tab Out or Reindent
通过tab键跳出(){}[]等。需要绑定键位配置，根据插件提示即可。

### Markdown Preview Enhanced
Markdown预览插件，实际上vs中提供了一个预览插件，但是不如该插件强大，例如默认预览插件无法显示目录。

- open preview to this side 在右侧打开一个预览窗口

### Markdown Table format
Markdown的表格格式化插件。在写完表格后执行该命令。

- format document / ctrl+shift+i 格式化文档

