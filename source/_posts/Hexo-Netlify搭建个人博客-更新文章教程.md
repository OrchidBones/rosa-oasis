---
title: Hexo+Netlify搭建&更新个人博客教程
date: 2025-08-26 14:42:09
categories:
- [教程, 编程]
tags:
- 代码
---
> 原教程[在此](https://zhuanlan.zhihu.com/p/39923772)。这篇教程精简了原教程的内容，并在其基础上添加了一些额外情况的设置。
>
> 注：以下列出的所有命令前面的 $ 符号表示 macOS 普通用户的命令提示符，Windows 用户在输入命令时不需要输入 $ 。

<br>

<br>

# Hexo+Netlify搭建&更新个人博客教程

主要思路：Hexo建站，Github托管代码，Netlify部署。

## I.搭建博客

### 一、安装前置

1. **安装 Node.js**
   
   访问[Node.js官网](https://www.nodejs.com.cn/download.html)，下载长期维护版的安装包，随后根据指引安装。

2. **安装 Git**
   
   访问[Git官网](https://www.nodejs.com.cn/download.html)，下载安装包并按部就班安装。

3. **注册Github账号**
   
   访问[Github官网](https://github.com/)，用邮箱注册一个Github账号。记住注册所用的邮箱和账号的用户名。

4. **注册Netlify账号**
   
   访问[Netlify官网](https://www.netlify.com/)注册一个账号。

5. **安装 Hexo**

   调出命令提示符/打开终端，输入npm命令安装Hexo：

   ```bash
   $ npm install -g hexo-cli
   ```

   安装完成后可以输入以下命令检查是否安装妥当：

   ```bash
   $ hexo -v
   ```
   
   如能显示hexo的版本和npm的版本，即安装成功。

<br>

### 二、搭建Hexo

1. **创建Hexo项目**

   输入以下命令创建项目文件夹路径：

   ```bash
   $ hexo init /PATH/TO/hexo
   ```

   <span id="jump"></span>切换至刚刚创建的文件夹。这里的路径建议为绝对路径：

   ```bash
   $ cd /PATH/TO/hexo
   ```

   **注意**！如果项目文件夹不在C盘，则输入命令（路径必须为绝对路径，例如 F:\Random\Blog）：

   ```bash
   $ cd /d /PATH/TO/hexo
   ```

2. **初始化项目**
   
   运行了刚才的 `hexo init` 命令，我们的项目文件夹内已经安好了必要组件。现在，通过npm来初始化Hexo：

   ```bash
   $ npm install
   ```

   等待搭建完成。这样，我们的网站就建好了。

   使用以下命令预览我们的网站：

   ```bash
   $ hexo server
   ```

   随后访问命令提示符或终端给出的网址即可预览。网址一般是 http://localhost:4000/ 或者 http://0.0.0.0:4000/ 。

   若要继续设置，在窗口焦点处于命令提示符 / 终端时，按 Ctrl+C / Command+C 。

   在这之后，可以设置基础参数、安装Hexo主题、撰写新博文。具体操作方法[见下](#II.更新博客)。

3. **建站前最后的准备工作**
   
   清除缓存数据，并为项目生成静态文件用以上传：

   ```bash
   $ hexo clean
   $ hexo generate
   ```

   鉴于我们不需要对网站内容进行版本控制，我们可将该文件夹添加至.gitignore文档中：

   ```bash
   $ echo "/public" >> .gitignore
   ```

   接下来，我们要将网站内容推送至Github这个代码托管服务上。

<br>

### 三、搭建网站

1. **配置Git**

   如果是第一次安装Git，那么在使用前，我们首先需要配置Git的基本信息，这些信息包括我们的Github账户用户名和注册邮箱。
   
   以用户名 `YourName`，邮箱 `YourEmail@example.com` 为例，打开命令提示符/终端，输入以下命令：

   ```bash
   $ git config --global user.name "YourName"
   $ git config --global user.email "YourEmail@example.com"
   ```

   配置好Git之后，可以用以下命令检查是否已成功安装：

   ```bash
   $ git --version
   ```

2. **<span id="deploy">开始托管</span>**
   
   登录Github账号，点击绿色按钮 `New`（Repository）新建一个仓库。为仓库命名时记住这个名字，后面会用。以下将会以 `hexo_netlify` 作为示例仓库名。

   注意，不要在 `Initialize this repository with a README` 前打勾，`Add .gitignore` 和 `Add a license` 处请选择 None。

   随后，在命令提示符或终端里切换到项目文件夹（[方法见此](#jump)），初始化仓库：

   ```bash
   $ git init
   ```
   通过 `git add` 跟踪指定项目文件，然后执行 `git commit` 提交：

   ```bash
   $ git add .
   $ git commit -m "initial commit"
   ```

   回到之前我们创建GitHub仓库完成的页面，复制远程仓库链接（一般长这样：https://github.com/yourName/hexo_netlify.git ），在命令控制符/终端输入：

   ```bash
   $ git remote add origin <远程仓库链接>
   ```

   可以用 `git remote -v` 来验证链接是否正确。

   确认准确无误后，用以下指令推送本地仓库内容至GitHub：

   ```bash
   $ git push origin master
   ```

   等待上传完成。

   至此，源代码就全部上传到Github了。接下来我们开始通过Netlify来发布网站。

3. **发布网站**
   
   登录Netlify账户，进入[建站页面](https://app.netlify.com/account/sites)，点击 `New site from Git` 进入下一步。

   首先，在第一步里选择 `Github`，按部就班关联 Github 账户。

   如果是第一次关联两个账户，则点击绿色按钮 `Authorize netlify` 同意授权Netlify访问GitHub上的仓库内容。
   
   授权完毕后，页面会跳转回Netlify，来到第二步。在这里选择我们刚才新建的Github仓库。

   然后来到第三步，我们只需将 `Build command` 参数设置为 **hexo generate** 即可。

   最后，点击 `Deploy site`，等待构建完成即可。

   <br>

   Netlify会在网站发布成功的同时提供给你一个以 *.netlify.com 为后缀、随机生成的英文名为前缀的子域名。假如你不喜欢Netlify自动生成的子域名，并且还未来得及购买域名，可以通过 `Domain settings` > `Edit site name` 来重命名。

<br>

<br>

## II.更新博客

包括设置、美化、更新博客的一系列操作。

### 一、设置基础参数

打开项目文件夹，用代码编辑器（推荐 NotePad++ 或者 VS Code）打开 `_config.yaml` 文件。

在靠前的部分，我们可以看到以下设置：

```
# Site
title: Hexo
subtitle: ''
description: ''
keywords:
author:
language: en
timezone: ''
```

其中，`title` 是网站名称，`subtitle` 是副标题，`author` 是建站者姓名，`language` 是网站的语言（简体中文是 zh-CN）。如有需要请自行修改。

<br>

### 二、安装主题

1. **安装**
   
   从Github上搜索并下载心仪的主题，随后根据其介绍页的教程安装。

   通过命令控制符/终端安装时，记得先切换至项目文件夹再安装。

2. **配置**
   
   打开项目文件夹，用代码编辑器（推荐 NotePad++ 或者 VS Code）打开 `_config.yaml` 文件。
   
   搜索关键词“theme”，将默认的 landscape 更改为安装好的主题的名字即可。

   若希望在主题上做进一步的修改，则在项目文件夹根目录中打开名为 _config.[theme-name].yaml 的文件，修改其中的设置即可。

<br>

### 三、撰写新内容

1. **撰写新博文**
   
   - **a. 生成博文文件**
   
   在命令控制符/终端里，先切换到项目文件夹下，然后输入命令：

   ```bash
   $ hexo new "新博文"
   ```

   这样，在项目文件夹下 source/_post/ 就会出现一个 新博文.md 文件。在这个新生成的 Markdown 文件里接着已经生成的内容撰写博文即可。Markdown 语法见[此处](https://www.runoob.com/markdown/md-tutorial.html)。

   如果我们不希望我们的文章发布出去，可以先生成一个草稿。所有草稿内容都不会被发布出来。

   在命令控制符/终端里，先切换到项目文件夹下，然后输入命令：

   ```bash
   $ hexo new draft "新博文草稿"
   ```

   这样，在项目文件夹下 source/_draft/ 就会出现一个 新博文草稿.md 文件。

   <br>

   - **b. 配置博文**

   打开新生成的.md文件后，我们可以看到里面已经有了一些内容：

   ```
   ---
   title: 新博文
   date: 20xx-xx-xx xx:xx:xx
   tags:
   ---
   ```

   这里的 `title` 就是博文的标题，`date` 是发布日期，`tags` 则是该博文的所属的一系列tag，可以自定义。

   如果想为博文添加多个tag，我们可以这样编辑 `tags` 字段：

   ```
   tags:
   - 标签1
   - 标签2
   ...
   ```

   除了以上默认的三个参数，我们还可以写以下这些参数：

   ```
   updated: 更新时间，格式和date相同
   categories: 所属分类
   excerpt: 文章摘要
   lang: 文章语言
   published: 是否发布
   author: 文章作者
   keywords: 关键词
   description: 文章简介
   ```

   **※** `categories` 和 `tags` 的相同点与区别：

   - 设置 `categories` 和 `tags` 时格式几乎一样。`categories` 也可以这样设置：

      ```
      categories:
      - 类别1
      - 类别2
      ```

   - 然而，`categories` 可以设置多级结构，`tags` 不行。例如，`categories` 可以这样设置：

      ```
      categories:
      - [类别1, 类别2]   // 属于类别1/类别2，类别2是类别1的子类别
      - 类别3
      ```
   
2. **添加新页面（Page）**
   
   在命令控制符/终端里，先切换到项目文件夹下，然后输入命令（例）：

   ```bash
   $ hexo new page about
   ```

   这样，在项目文件夹下 source/about/ 就会出现一个 index.md 文件。编辑此.md文件即可。

   在我们用 `hexo server` 预览网站时，访问 ./about/就可以看到在index.md中编辑的内容了。

<br>

### 四、更新博客

在命令控制符/终端里，先切换到项目文件夹下，然后清除旧缓存数据并生成静态页面：

```bash
$ hexo clean
$ hexo generate
```

启用本地服务器预览更新后的网站：

```bash
$ hexo server
```

确认无误后退出预览，开始部署到线上。

此处基本是重复 I.搭建博客 > 三、搭建网站 > 2.开始托管（[点击此处](#deploy)）中的命令操作，但略有不同。

首先初始化Git，然后通过 `git add` 跟踪指定项目文件：

```bash
$ git init
$ git add .
```

如果记得自己修改、增添、删除了什么文件，也可以直接 add 指定文件。不过这么做比较麻烦。

然后执行 `git commit` 提交：

```bash
$ git commit -m "new post added"
```

这里的 `"new post added"` 可以认作是此次更新的更新信息，可自定义。

最后直接用以下指令推送本地仓库内容至GitHub：

```bash
$ git push origin master
```

等待上传完成即可。