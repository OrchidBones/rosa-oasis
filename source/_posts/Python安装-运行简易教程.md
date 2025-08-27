---
title: Python安装+运行
date: 2025-08-27 15:17:06
categories:
- [教程, 编程]
tags:
- 代码
---
# Python安装+运行简易教程

在Windows系统上安装 Python 3 并在 VS Code 终端运行教程

仅面向外行人或只想用Python处理简单任务的人（比如我）。

请先提前安装好 VS Code。安装包可以在[官网](https://code.visualstudio.com)下载。安装后记得安装简体中文的扩展插件。

<br>

## 一、下载并安装Python3

1. 下载安装包。

    访问[官网](https://www.python.org/downloads/)并下载适用于电脑系统的最新版本的 Python 3 安装包（一般情况下下载 64-Bit 的版本即可）；

2. 点击安装包开始安装。

    在弹出的安装窗口的第一页，勾选最下方的“Add Python 3.x to PATH”，然后选择“Install Now”进行下一步安装。

    按部就班完成接下来的安装步骤之后，等待 Python 安装完成。

3. 测试 Python 是否已经成功安装。

    按Win+R，输入cmd调出命令提示符，然后输入“python”，按回车。如果能够看到Python的相关信息（版本等），则说明安装成功。

<br>

## 二、设置环境变量

调出命令提示符，输入：

```
path=%path%;[PATH]
```

[PATH] 是 Python 的安装目录。

例如：

```
path=%path%;C:\Python
```

<br>

## 三、配置 VS Code

1. 安装 Python 对应的扩展插件。
   
   在 VS Code 左侧边栏选择“扩展”，搜索、下载并启用 Python、Pylance、Python Debugger、Python Environments 四个插件。

2. 设置解释器。
   
   在上侧工具栏里选择“文件（Files）”，再选“打开文件夹（Open a Folder...）”，随意选择一个文件夹（可以是桌面），然后新建一个 .py 文件。按 Ctrl+Shift+P 调出命令面板，搜索 “Select Interpreter”，选中，然后选择“Python 3.x.x”（一般位于最后一个）。

<br>

## 四、在终端运行 Python

写好 Python 代码后，右键调出快捷菜单，鼠标移到“运行 Python（Run Python）”，然后点击“在终端中运行 Python（Run Python in terminal）”即可。

<br>

这样就可以方便地处理任务了。