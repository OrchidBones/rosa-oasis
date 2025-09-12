---
title: Web打包APK保姆级教程
date: 2025-09-12 23:05:18
categories:
- [教程, RM教程]
tags:
- 代码
- RMMV
- RMMZ
---
# Web打包APK - Website 2 APK Builder 使用教程

*自用存档

<br>

※ **系统**：Windows

※ **所需环境**：JDK 环境 (版本 15.0.2) [点我下载](https://pan.baidu.com/s/1_jqux6ug7Rrz3GGaR-el2A?pwd=java)

※ **所需软件**：Website 2 APK Builder v5.0 [点我下载](https://pan.baidu.com/s/1Bdl7QnjPphWy5rWwgbqPhw?pwd=d66z)

<br>

## 一、准备工作

### 1、本地网页源码的处理

※ 如果你打包的是一个网页 url，请跳过。

（1）绝对路径在打包好的 APK 中不起作用，这是因为绝对路径会将 `file:///` 作为根目录，而我们的项目根目录实际上是 `file:///android_asset/` 路径下。解决方法是将所有的绝对路径中开头的“/”替换成“`file:///android_asset/`”，或使用相对路径。

（2）每一个路径都必须导向一个 .html 文件。不能只写路径。

### 2、安装JDK并配置环境变量

※ 已经安装好的请跳过。如果你安装的是 16 及以上的版本，请卸载掉高版本 JDK 并下载版本 15 的 JDK，因为 Website 2 APK Builder v5.0.0 不支持用 版本 16 及以上的 JDK 生成的 Keystore。

（1）下载 JDK15 压缩包并安装到一个好记的地方，例如 `C:\Program Files\Java\jdk-15.0.2\`。

（2）复制 JDK 的 bin 文件夹目录。以（1）中设置的为例，则是 `C:\Program Files\Java\jdk-15.0.2\bin\` 。

（3）右键点击“我的电脑” » 点击上下文菜单中的“属性” » 搜索“查看高级系统设置”，点击搜索结果 » 点击窗口右下角“环境变量” » 点击下半部分窗口的 Path » 添加环境变量，将在（2）中复制的路径粘贴到输入框，点击确认。现在 JDK 理论上已经配置好了。

（4）若想确认 Java 环境是否已经成功配置，请按 Win+R 组合键，输入 cmd 打开命令提示符，输入 java 后按回车。如果有显示一长串很多行的数据和参数，就说明配置完成。

### 3、安装 Website 2 APK Builder v5.0.0

（1）下载压缩包并解压，点击 Website.2.APK.Builder.v5.0.Installer.exe 安装软件。安装时不要勾选安装完自启动选项。

（2）用（1）中解压的 crack 文件夹中的 Website 2 APK Builder Pro v5.0.exe 替换 Website 2 APK Builder 的安装目录中的同名文件。

（3）点击安装目录中的 Website 2 APK Builder Pro v5.0.exe 即可运行。

<br>

## 二、构建 APK

### 1、创建签名证书 (*.keystore)

如果 APK 没有签名证书，用户手机很可能就会拒绝安装。

（1）点击电脑左下角打开开始菜单，在 “Windows 系统”选项卡中右键点击“命令提示符” » 点击“更多...” » 点击“以管理员的身份运行”，打开命令提示符；

（2）输入命令：

```bash
cd {PATH}
```

将 {PATH} 更改为 Java 安装的 bin 文件夹路径。例如可以输入：
```bash
cd C:\Program Files\Java\jdk-15.0.2\bin
```
然后按回车；

（3）输入命令：
```bash
keytool -genkey -alias YOUR_ALIAS -keyalg RSA -validity 36000 -keystore YOUR_KEYSTORE_FILENAME -storepass YOUR_STORE_PASSWORD
```
`YOUR_ALIAS` 是 **签名别名** (Alias)，`-validity` 后面的数字指的是该证书有效期为 36000 天，`YOUR_KEYSTORE_FILENAME` 是生成的 .keystore 文件名（需要带扩展名），`YOUR_STORE_PASSWORD` 是 **签名密匙库口令**，一定要记好，给 APK 签名时会用到。设置好后按回车。

（4）之后按照窗口上给出的问题回答就行。最后一个问题（国家代码）可以填 CN，其他的问题随便填就行。

（5）当问到确定使用以上设置时，输入 y ，Java 的 bin 文件夹里就会出现 .keystore 文件。

（6）为了确保证书无误，可以继续在窗口中输入：
```bash
keytool -list -keystore "YOUR_KEYSTORE_FILENAME"
```
`YOUR_KEYSTORE_FILENAME` 是生成的 .keystore 文件名（需要带扩展名）。随后输入**密匙库口令**，如果已经配置好，就可以看到指纹等一系列属性。

### 2、配置项目

（1）打开 Website 2 APK Builder，依次设置参数。以下是常用参数翻译与解释：

- `Website type to convert`: 如果是本地 Web 项目，就选 Local HTML Website，如果只是打包一个网址就选 Web URL。

- `App Title`: 你的 APP 的名称。

- `Package Name`: 你的 APP 的包名。APP 的包名就相当于 APP 的身份证，必须是独一无二的。包名的必须由小写字母、-和_组成，通常是倒过来的网址，但也可以是别的词组组合，例如 owl.potter.harry 。

- `Version Name`: 版本名称。仅用于对用户显示当前 APP 的版本，格式任意，但建议是数字。

- `Version Code`: 版本代码。初始值是 1。无论版本名称是怎样的，但凡你的 APP 得到了升级，就必须增加版本代码的值。因为当用户重新安装 APP 时，会通过比较新 APP 和旧 APP 的版本代码以判断是否用新 APP 覆盖旧 APP，若新 APP 的版本代码大于旧 APP 的版本代码，则覆盖，相当于为这个 APP 做一次更新 / 升级。

- `App Orientation`: APP 朝向。`Auto Rotate` - 自适应旋转，`Portrait` - 竖屏，`Landscape` - 横屏。

- `Output Directory`: 导出路径。导出 APK 将会出现在这一路径下。

- `About Dialog Text`: 顶部菜单栏“About”的文本。

- `App Share text`: 顶部菜单栏“Share”的文本。

- `App Exit Text`: 退出 APP 时的文本。

- `Change Icon`: 更改 APP 的应用图标。

- `Customize App`: 自定义 APP 的权限等设置。

其他功能可以自行设置。

（2）（1）中的参数配置完后，点击菜单栏的 `Keystore & Signing`，再点击 `Custom Keystore (Most Recommended!)` 以导入自定义的签名证书。以下是翻译与解释：

- `Select Keystore`: 找到刚刚生成好的 Keystore 文件。

- `Password`: 输入密匙库口令。

- `Alias Name`: 输入配置好的别名 (alias)。

- `Alias Password`: 再输入一次密匙库口令。

之后点击 `Import & Apply`，如果弹出 Applied successfully 的对话框，就说明导入成功。

（3）点击主页的 `Build Android APK`，等待片刻即可在导出目录中看见构建好的 APK。