---
title: 记一个ie下特有的代码情况
date: 2019-12-30 17:16:40
tags: 前端
---

# 起因

起因是接到了一个bug，说是代码在ie下运行，无法正常运行。当时想了一下，那不是理所当然的嘛，没有接babel-polyfill，所以打不开是正常的一踏糊涂，代码里还有不少Promise相关的代码。

### 结果出现了反转，他们说打开了f12（控制台）以后，跟我说界面可以正常加载出来了。

我当时就？？？？？？？了

![一懵逼](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/common/yimengbi.jpeg)

然后我喊上了我的同事，一起来围观ie11下的神秘问题。

然后他表示

> 这是我工作以来碰到的最神秘的bug

![对懵逼](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/common/mengbi.jpeg)

好了，这下懵逼的人又多了一个

# 调查

既然出现了这么有趣的问题，那怎么可以放过它呢。。

通过在google上搜索关键字 ie11 不开 调试器 查到了一个很相似的网页内容

![ie11不开调试器无法打开](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/common/thinbug.jpg)

救星啊！！曙光啊！！女神啊！！

然后点了进去

![女神不见啦](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/common/502.jpg)

女神他门坏啦！！！女神他穿上了坦克装甲！！！

不过幸好的是，我们找到了关键字，所以我们继续按照这个页面的关键字进行检索，他已经给出了大方向

我们将关键字修改为 

> ie11 调试器 除非启动

果不其然，正确答案出现了(按F进入坦克)

# 正确答案

> https://xbuba.com/questions/44036862

在这篇文章中，将这个问题的关键描绘了出来

> 脚本错误仅在以下情况下才会执行：1.打开浏览器调试工具。和2.已打开开发人员工具的“异常中断”的“调试”选项卡设置。

这个词点醒了我，通过在ie11的调试菜单栏中，果不其然发现了一个调试选项是修改ie11是否碰到异常后进行中断的

这个选项，在不打开调试控制台(F12)的默认对情况下是会进行中断的，所以我们看到的ie11上页面为无法渲染，但是在打开调试控制台后，它的选项就会修改为异常后不进行中断

而代码中，因为业务代码对异常进行了捕获，但是在最后没有进行抛出，所以形成了这次无法理解的bug

通过将其设置为异常时中断，原始问题脱下了它的坦克装甲

> Promise.resolve 无法对null使用resolve方法

归根到底还是未引入Promise导致的问题。。绕了好大一个圈，最后的解决办法竟然上莽上去就好了。。。

### 最后，通过引入babel-polyfill的办法，彻底解决了这个问题，顺便给ie做了个全套大保健的兼容

# 结语

写了这么多，写着写着就写成了如何正确使用搜索引擎来解决bug。。。不过我觉得这个确实相对于单纯的解决问题更重要，毕竟大部分的别人的问题，到了我这里，我也只是换了个姿势进行了搜索而已。。

授人以鱼不如授人以渔，大家好好学习百度的一百种姿势呀