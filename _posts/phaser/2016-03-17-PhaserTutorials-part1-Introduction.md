---
layout : post
title:  phaserTutorials 介绍
category : phaser
duoshuo: true
date : 2016-03-17
---

<!-- more -->

## 简介

In this tutorial we're going to cover setting-up a development environment with which you can build your Phaser games.
在本教程中，我们将介绍如何建立一个开发环境来开发你的Phaser游戏的。
This will include running a local web server, picking an IDE, getting the latest version of Phaser and checking it all works together.
介绍的内容包括启动一个本地WEB服务，选择一个IDE，下载最新版本的Phaser，并确保他们能够一起工作。
If you trust us that you do need a local web server for development, then you can skip the explanation below and head directly to part 2.
如果你相信我们并且你需要一个本地服务来开发，那么你可以跳过下面的解释和直接开始第二部分。
If you'd like to know the reasoning why, please read on ...
如果你想知道为什么，请阅读…

## A web server? But I want to make games!
一个网络服务器？但我想做游戏！

"Why do I need a local web server? Can't I just drag the html files onto my browser?"
“为什么我需要一个本地的网络服务器？我不能把HTML文件拖拽到我的浏览器吗？”
                                                               A. SANE, DEVELOPER
                                                               理智，开发者
Not these days, no. I appreciate that it's a bit confusing,  even contradictory at times, but it all boils down to browser security.
不止这些天，我明白这有点混乱 。不， 甚至一直是矛盾的，但它都归结为浏览器的安全性。
If you're making a static html web page then you can happily drag this file into your browser and see the end results.
如果你正在做一个静态的HTML网页，然后你可以愉快的将这个文件拖到你的浏览器，看到最终的结果。
You can also "Save As" entire web pages locally and re-open them with most the contents intact.
您还可以“保存为”整个网页，并将其重新打开以最完整的内容来显示
 If both of these things work why can't you drag an HTML5 game into a browser and run it?
 如果这两种事情都有效，你为什么不能拖动一个HTML5游戏到浏览器并运行它？

It's to do with the protocol used to access the files.When you request anything over the web you're using http,
当你使用HTTP服务请求信息时，它是通过文件传输协议进行的。
and the server level security is enough to ensure you can only access files you're meant to.
并且服务器级别足以保证你可以安全的访问你想要的文件。
But when you drag a file in it's loaded via the local file system (technically file://) and that is massively restricted, for obvious reasons. Under file:// there's no concept of domains, no server level security, just a raw file system.
但当你拖动文件加载到本地文件系统（从技术上说：file//）并且他是有限制的，明显的原因： 根据 file:// 没有域的概念，没有服务器级别的安全性，只是一个原始文件系统。

Ask yourself this: do you really want JavaScript to have the ability to load files from anywhere on your local file system?
问问你自己：你真的想让JavaScript 获得从你本地文件系统中加载任何文件的能力？

Your immediate answer should of course be "not in a million years!". If JavaScript had free reign while operating under file:// there would be nothing stopping it loading pretty much any file, and sending it off who knows where.
如果Javascript在file://下拥有自由的操作权限并且不停的载入相当多的文件发给你并不知道的地方,你会直接的回答："绝对不会" 。
Because this is so dangerous browsers lock themselves down tighter than Alcatraz when running under file://.Every single page becomes treated as a unique local domain.
因为在file:// 下浏览文件是很危险的。每一个页面都被视为一个独特的本地域。
That is why "Save Web page" works, to a degree. It opens under the same cross-site restrictions as if they were on a live server.
这就是为什么“保存网页”会起作用，到一个程度。它们是在一个有效的服务器交叉站点的限制下打开。
There's a detailed post about it on the Chromium blog that is well worth a read if you'd like to learn more.
如果你想学更多的话，本博客上详细的帖子值得一读的。
Your game is going to need to load resources: images, audio files, JSON data, maybe other JavaScript files. And in order to do this it needs to run unhindered by the browser security shackles. It needs http:// access to the game files. And for that we need a web server.
你的游戏需要加载资源：图片，音频文件，JSON数据，也许其他JavaScript文件。为了这样做，它需要运行顺畅的浏览器安全的枷锁。它需要HTTP://进入游戏文件。我们需要一个网络服务器。