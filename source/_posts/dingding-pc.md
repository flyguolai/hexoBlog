---
title: 网页端通过url直接打开pc版钉钉
date: 2020-01-20 15:23:10
tags: 前端
---

[钉钉统一开发协议](https://ding-doc.dingtalk.com/doc#/dev/tn8bih)

## PC端统一跳转协议（我照抄的）
html页面填写钉钉PC端指定链接，当用户点击链接时，唤起PC客户端，并执行相应操作。

如打开个人profile页链接：

<a href="dingtalk://dingtalkclient/action/sendmsg?dingtalk_id={id}"></a>


打开个人profile页，在站点挂个人钉钉联系方式，点击打开个人profile页，步骤如下：

第一步，确认钉钉号：在钉钉移动客户端“我的”，打开"我的信息"页面下查看，钉钉号为xxxxxx；


第二步，替换{id}，替换后链接为

> dingtalk://dingtalkclient/action/sendmsg?dingtalk_id=xxxxxx；

第三步，打开个人profile页示例：PC端打开个人profile页，点击钉钉联系我。



注意: PC端统一跳转协议暂时不支持服务端获取 dingtalk_id，若页面在钉钉PC端容器内执行，可尝试使用 biz.util.open（需要改跳转链接）JSAPI打开个人资料页。


## 如何实现客户端注册urlScheme

实现原理：
window是把自定义协议写入注册表，打开对应exe程序
mac是在Info.plist文件添加CFBundleURLTypes

参考文章：
[实现网页调起PC客户端，打开会话聊天](https://blog.csdn.net/weixin_34245082/article/details/89141998)