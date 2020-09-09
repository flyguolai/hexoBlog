---
title: 关于webgl的共享context
date: 2020-09-09 16:51:14
tags:
---

# 共享context的调查


### 期望：与opengl实现组相同，使用共享context资源来实现优化，避免代码冗余（2套实现标准）

### 背景：经过技术组调查，发现webgl 2 以 opengl es 3的为目标实现了很多特性，故调查webgl2是否支持opengl es 3的shareGroup特性

### 过程

首先根据百度进行搜索共享context，基本没有什么有用的消息

继而搜索webgl 2 新实现的一些特性，依旧无果，得到了别人的一些调研结果，貌似webgl2对于实现并不积极的结果



得到WebGL部分文档与demo



（知乎回答）https://www.zhihu.com/question/41447132?sort=created

（demo）https://link.zhihu.com/?target=https%3A//github.com/WebGLSamples/WebGL2Samples

（webgl文档）https://www.khronos.org/registry/webgl/specs/latest/2.0/


（opengl es 3文档）https://www.khronos.org/registry/OpenGL/specs/es/3.0/es_spec_3.0.pdf#nameddest=section-3.7.1

继而进行谷歌，依旧无果

寻求c++开发帮助，得到开发文档*1

https://www.khronos.org/webgl/wiki/SharedResouces

发现这只是一个目标，其中只写了部分理想的实现标准，并没有找到相关有效的资料

根据`webgl2 + SharedResouce`进行搜索

发现

https://www.khronos.org/registry/webgl/extensions/rejected/WEBGL_shared_resources/



看到标题里带了rejected，心里咯噔了一下



仔细阅读，阅读到最后


```
Revision 1, 2012/02/06

Initial revision.

Revision 2, 2013/05/14

Moved to draft after agreement in working group.

Revision 3, 2014/07/15

Added NoInterfaceObject extended attribute to extension interface and WebGLShareGroup.

Revision 4, 2018/05/02

Rejected after discussion on public_webgl and no strong objections. At this point in the WebGL API's development it is not profitable to invest the time to implement this extension. Alternatives: CanvasRenderingContext2D.drawImage taking a WebGL-rendered canvas as argument, or using OffscreenCanvas.transferToImageBitmap with a WebGL-rendered canvas, combined with ImageBitmapRenderingContext.transferFromImageBitmap.
```


于`2012`年提出的草案，但是在`2018`年驳回了

当然khronos给的理由是这个东西价值不大，我们没那么多人投到这个东西里开发，如果要使用的话可以用类似于drawImage的形式去实现（翻译一下：我们没空搞这个，这个性价比太低了，你们可以用离屏canvas画2d的行为替代3d数据共享的行为，先将就用这个吧）

最后结果是好的，使用他们提出的2d去读3d显示的画面效率很可以，毕竟调研的目的是能否公用一个context来实现渲染，最后实验通过了

### 发现了效率问题

虽然在直接使用的过程中发现无法正常将一张canvas上的东西画到另一张上去，但是在一通stackoverflow后，得到了答案https://stackoverflow.com/questions/31710748/todataurl-of-webgl-canvas-returning-transparent-image

在第一次getContext时，配置项加入{preserveDrawingBuffer:true}

需要对上一次绘制的结果进行preserve

但是个人觉得这个api并不友好，虽然是getContext，其实本质上也对context进行了一次初始化，并且之后如果和这次初始化的类型不一样只是会返回null，并没有很好的语义化，用起来特别别扭

最后尝试使用offscreenCanvas，它本身提供了drawBitMap的api，相对于显式的canvas友好不少

奇葩的是offscreenCanvas的更新需要调用它的toBitmap接口才好……不然它都是惰性的更新视图……黑一会更新一下黑一会更新一下……

## 2020/8/24 更新

还是不行，offscreen的操作明显是有惰性绘制操作的，视图更新不及时，同时兼容性(chrome 69,safari 全红)实在是太差了……

最后在通过js生成一个canvas然后直接draw到页面上……效率挺高的，完美

### 总结：

使用同一个context的形式可行，通过生成bitmap的形式效率在多个cnavas同屏幕显示效果也非常好（这行划掉）

还是通过显式的canvas来更新canvas吧，offscreenCanvas不太行……

## 2020/9/9 更新

共享context有背景色无法正常显示问题，故还是直接渲染到页面上……