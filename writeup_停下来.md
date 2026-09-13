---
layout: post
title: "Bugku 你必须让他停下 Writeup"
date: 2026-09-13 11:00:00 +0800
tags: [CTF, Bugku, Web]
categories: CTF_Writeup
---

Bugku 你必须让他停下 Writeup

一、题目信息
做题平台：Bugku CTF
题目类型：Web基础
靶场地址：http://160.202.254.160:14428

二、题目分析
打开链接之后页面一直在自动刷新，根本来不及看页面上的内容。
这道题提示是“你必须让他停下”，猜测是前端JS代码在控制页面不停重载，flag藏在某一次刷新返回的页面源码里面。

三、解题思路
页面持续刷新是JavaScript写的自动重载代码。
最简单的办法，用浏览器开发者工具关掉JS，页面就不会继续刷新了；
也可以用Burp抓包反复发送请求，多试几次，抓到带有flag的返回包。

四、详细步骤
1. 打开靶场网页，按下F12调出开发者工具。
2. 按快捷键Ctrl+Shift+P，在弹出的输入框搜索javascript，选择禁用JavaScript。
3. 关掉JS之后，页面就停止刷新了。
4. 多次访问页面查看源码，直到找到包含flag的内容。

五、最终Flag
flag{dummy_game_1s_s0_popular}

六、总结
这道是Web入门题，主要考前端JS相关知识。
学会禁用JS、抓包拦截请求，就能处理这种页面自动刷新的场景。
