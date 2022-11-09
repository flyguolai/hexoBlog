---
title: auto-complete Chrome浏览器下异常的调研
date: 2022-11-09 11:27:18
tags: Browser 浏览器 文档
---

之所以会有这篇文章，是在做表单的时候被自动表单填充给折磨到了

明明只是填了一个Address，会直接跳出来相关的mail和发送至mail的Code（为啥会有这种神秘的联动），虽然可以在浏览器中关闭地址的保存，但是用户可不知道这些，导致使用起来mail的code自动填充很蠢

为了解决这个问题，想了两个方案
1、修改form中input的name属性值为其他，通过让chrome识别不到对应的联动字段来解决该问题
2、学习autoComplete属性规则，通过修改autoComplete为chrome能识别到的值来取消掉自动填充的行为

由于字段名的name属性是与提交到后端的字段同步的，所以无法直接通过修改name属性的字段名来达到取消自动输入的效果，所以方案一采用失败，使用方案二

正确的做法应该是通过修改form表单或者input输入框的autoComplete的值来修改自动输入的行为

但是由于chrome自我的实现（这个feature/bug一直是chrome的一个争议），它会忽略掉autoComplete='off'这个值，强行让自动输入存在（网上收集到的资料显示国产的搜狗浏览器比这个更过分，不管填啥，它都会强制开启autoComplete），所以需要修改autoComplete的值为nope，这个浏览器根本不认识的值，来规避掉自动填写内容


相关链接：
https://copyfuture.com/blogs-details/202205230542221013
https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete