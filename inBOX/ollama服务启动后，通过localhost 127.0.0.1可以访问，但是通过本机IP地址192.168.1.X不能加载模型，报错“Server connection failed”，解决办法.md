---
title: "ollama服务启动后，通过localhost 127.0.0.1可以访问，但是通过本机IP地址192.168.1.X不能加载模型，报错“Server connection failed”，解决办法"
source: "https://zhuanlan.zhihu.com/p/2990519424"
author:
  - "[[知乎专栏]]"
published:
created: 2025-02-05
description: "问题现象： ollama服务启动后，通过localhost:3000和 127.0.0.1:3000可以访问并加载模型，但是通过本机IP地址192.168.1.6：3000不能加载模型，报错“Server connection failed”。 原因： 跨域访问造成的。如果是…"
tags:
  - "clippings"
---
问题现象

 ollama服务启动后，通过localhost:3000和 127.0.0.1:3000可以访问并加载模型，但是通过本机IP地址192.168.1.6：3000不能加载模型，报错“Server connection failed”。

原因：

[跨域访问](https://zhida.zhihu.com/search?content_id=249620724&content_type=Article&match_order=1&q=%E8%B7%A8%E5%9F%9F%E8%AE%BF%E9%97%AE&zhida_source=entity)造成的。如果是本机访问，ollama默认允许本机跨域访问（官方文档说**修改系统变量OLLAMA\_ORIGINS即可解决非本机跨域问题**（windows 下）。

解决办法：

1、在系统变量中添加变量：OLLAMA\_HOST，变量值：0.0.0.0

2、添加OLLAMA\_ORIGINS，变量值：\*

3、终端窗口退出ollama webui，托盘退出ollama

4、重启ollama软件，重启ollama webui(npm run dev)

刷新浏览器即可。