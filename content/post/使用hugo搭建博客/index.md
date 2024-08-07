+++
title = "使用hugo搭建博客"
date = 2024-08-06T19:39:48+08:00
categories = "学习笔记"
image = "cover.png"
+++ 
## 前言
搭个博客各种问题改来改去好麻烦，简单记录一下做到目前这个程度的路径吧。
## 使用Hugo本地建站
### 什么是Hugo
[Hugo](https://www.gohugo.org/)是由 Go 语言实现的静态网站生成器。简单、易用、高效、易扩展、快速部署。   
下面内容参考hugo官网的[quick start](https://gohugo.io/getting-started/quick-start/)
### Hugo本地安装
可以参考[官方文档](https://gohugo.io/installation/),我是win11系统，这里使用[Chocolatey](https://gohugo.io/installation/)安装了hugo的拓展版本:
```
choco install hugo-extended
```
Chocolatey 是一个流行的软件包管理器，主要用于 Windows 操作系统。它受到 Linux 和 Mac 系统中已有的包管理工具（如 apt、yum 和 Homebrew）的启发，旨在简化在 Windows 平台上安装和更新软件的过程。这个可以自行学习使用。

安装完成后，输入以下命令检查hugo版本：
```
hugo version
```

### 使用hugo创建博客
输入下列指令以创建博客项目：
```
hugo new site [项目名称] 
```
该命令会在目录下创建博客项目文件夹，输入指令将其该为当前根目录
```
cd [项目名称]
```
这里我是用的主题是stack，将其克隆到themes文件夹
```
git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
```
在网站配置文件夹`hugo.toml`文件后追加一行，指向当前主题
```
theme = '主题文件夹名称' 
```
启动Hugo的开发服务器查看站点
```
hugo server
```
如果运行成功，提示的倒数第二行会显示本地服务器地址，打开即可看到你部署的网页。

按`Ctrl + C`停止运行

## Stack主题美化
Stack是hugo中的一个主题，详细配置可以参考[Stack中文文档](https://stack-docs.netlify.app/zh/configuration/)

## 编写博客
### 使用hugo创建文章
在博客根目录下运行创建新博客文章:
```
hugo new post/untitled.md
```
比起直接新建`.md`文件，使用hugo创建可以自动填充[Front Matter](https://www.gohugo.org/doc/content/front-matter/)模板，模板可以在`\archetypes\default.md`中找到并修改

### 添加图片
众所周知`.md`文件是不存储图片的，想在文章中存储图片有三种方式:  

一种是将图片添加到静态资源中然后引用，这样编译后图片会存储在网站根目录以供调用，缺点是编写时没法查看图片，全凭感觉。

还可以在创建博客时多建一层，使文件夹结构如下    
* post
* * newBlog
* * - index.md
* * - img.png

这样则可以通过相对路径调用图片

还有就是通过图床来调用，这个可以稍后再学
