---
title: HonKit生成电子书
date: 2025-08-27 13:38:32
categories:
- [教程, 其他]
tags:
- 创作
- 阅读
---
# HonKit生成电子书简易教程

官方文档：https://honkit.netlify.app

<br>

## 1. 介绍

HonKit 是一个命令行工具，可将 markdown 文件编译成带目录的电子书并将其导出为 html, pdf, epub, mobi 格式。

HonKit 是 gitbook 的升级版本。介于 gitbook 已不再维护，gitbook 不支持高版本的 NodeJS。然而，HonKit 兼容最新版本的 NodeJS LTS，可以很好地替代 gitbook 工作。

<br>

## 2. 安装HonKit

进入NodeJS官网 https://nodejs.org/ 下载 NodeJS LTS 并安装。（建议安装版本 10.0.0 以上）

然后，使用Node.js自带的NPM工具安装HonKit。在命令控制符/终端窗口输入：

```bash
$ npm install -g honkit --save-dev
```

对于之后添加的插件，我们也需要使用 -g 安装到全局。

<br>

## 3. 创建一个新电子书项目

在命令控制符/终端窗口输入：

```bash
$ npx honkit init {DIRECTORY} 
```

{DIRECTORY}是项目所在的文件夹绝对路径。

构建完毕后打开项目文件夹，就会出现 `SUMMARY.md` 和 `README.md` 两个文件。

- **项目文件结构**
  
  项目中必须拥有 `SUMMARY.md`, `README.md` 和 `book.json`。
  
  `SUMMARY.md` 存放电子书的目录结构，按无序列表排列。
  
  `README.md` 记录电子书封面显示的内容。
  
  `book.json` 可为电子书进行配置。
  
  除此以外还可创建一个 `GLOSSARY.md` 文件，用来存放术语。
  
  若电子书有多种语言版本，可以创建一个 `LANGS.md` 文件，用来配置多语言设置。
  
  关于电子书配置选项及书写格式的详细内容，参见[官方文档](https://honkit.netlify.app/structure.html)或见下简短说明。

<br>

## 4. 设置&撰写电子书内容

a. **设置电子书属性**

在项目文件夹里新建一个 `book.json` 文件。

内容这样写：

```json
{
    "title": "我的电子书标题",
    "author": "作者名",
    "description": "这是这本电子书的描述。"
}
```

除了上面三个参数之外，还有其他参数可设置。详情参见[官方文档 - Configuration](https://honkit.netlify.app/config)。

b. **撰写电子书&设置目录**

下载一个 Markdown 文件编辑器，然后新建.md文件，在文件里编辑电子书的内容。

可以将电子书根据章节拆分出多个文件。

随后，将这些文件放入我们的项目文件夹里。可以放在不同文件夹里用来分类。

对于电子书的目录、各章节的配置和格式，可参见[官方文档 - Pages and Summary](https://honkit.netlify.app/pages)。

<br>

## 5. 预览电子书

在命令控制符/终端窗口输入：

```bash
$ npx honkit serve {DIRECTORY} 
```

`{DIRECTORY}` 是项目文件夹绝对路径。

等待加载完成后打开 http://localhost:4000 即可预览静态网页。

<br>

## 5. 生成电子书

在命令控制符/终端输入：

```bash
$ npx honkit build {DIRECTORY} 
```

{DIRECTORY}是项目文件夹绝对路径，即可生成配套的 html 静态网页。

除此以外，还可以生成 pdf, epub, mobi 文件：

- 生成 PDF 文件
  
  ```bash
  $ honkit pdf ./ ./mybook.pdf
  ```

- 生成 ePub 文件
  
  ```bash
  $ honkit epub ./ ./mybook.epub
  ```

- 生成 Mobi 文件
  
  ```bash
  $ honkit mobi ./ ./mybook.mobi
  ```