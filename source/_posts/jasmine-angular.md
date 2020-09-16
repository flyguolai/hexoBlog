---
title: jasmine-angular
date: 2020-09-10 15:25:28
tags: unit-test
---

# 使用angular自带的unit-test框架

## 引言

Angular集成了一个unit-test框架，比起react和vue那种自己找unit-test框架，有时候还会发现官方文档挂着的unit-test库已经不维护的蛋疼感要好不少，这也算是angular用起来的一个很大的优点了

Angular集成了的框架其实是jasmine，本身是可以脱离angular单独运行的，不过由于是angular主动集成的，所以angular提供了很多可以测试用的工具，以在jasmine中直接运行，这确实省了很多力气

## 基本使用方法

jasmine的基本使用很简单，和其他的几个单测框架差不多，主要就是分为生命周期，断言

[github地址](https://github.com/jasmine/jasmine)
[文档](https://jasmine.github.io/)

### Simple Sample Code

```javascript
describe("A suite is just a function", function() {
  var a;

  it("and so is a spec", function() {
    a = true;

    expect(a).toBe(true);
  });
});
```

### 基本结构

根据上面的官方提供的sample code，我们可以得到一个大致的结构

```javascript
describe("这里写单测的集合名", function() {
  // 这里一般是多个测试用例公用的变量的初始化

  it("这里写单条测试用例", function() {
      // 这里是具体的测试用例的执行
  });
});
```

### 断言

```javascript
expect(a).toBe(b);
```

上面这一句其实就是断言了。

断言的本质其实和js中的===差不多，用来做结果的判断。

当然他内部肯定是做了很多内部动作的，虽然我没看过（- -！

对于我们使用者来说，主要目的其实就是了解到断言的作用即可，以及使用断言可以比较好的产出报告

### 生命周期

为了使单元测试相对来说可控以及结构简单，jasmine提供了几个生命周期钩子

+ beforeEach
+ afterEach
+ beforeAll
+ afterAll

相对来说他的几个api的名字写的相当的好，基本上看名字就知道这几个api是用来干啥的了

### 亿些细节

相对来说文档写的很完善了，这里直接贴一个地址以供自己以后查阅

[jasmine入门](https://jasmine.github.io/tutorials/your_first_suite)

## 配合angular

其实如果只是测试简单的函数/逻辑，如果与界面无关的话，这样已经可以用了，但是前端的书写往往和界面框架绑定的比较严重，如果只是测逻辑的话，很多界面逻辑是无法获得其结果的，特别是angular这种还带有模版语法的，就会更加不可控，所以我们需要angular的一些工具来帮我们解决问题

[angular单元测试](https://angular.cn/guide/testing)

### 使用命令行工具来快速生成spec文件

如果在建立component和module等目录的时候使用的是angular-cli，那么在建立完文件夹后对应ts的文件下，都会有一个对应名字的spec文件，这个就是可以给单测框架识别到的文件，我们就直接在里面写测试用例

由于angular自身的文档对于单元测试写的是在是很细了（从Service开始啥类型需要注意什么都写了），所以这里便不展开了，直接看[angular的测试文档](https://angular.cn/guide/testing)即可


# 总结

之前花了大力气搞定了单测这部分，能让它正常跑起来，对于代码的可靠性检测还是很有意义的，单测写的好能极大改善我改了一个地方导致其他地方爆炸的问题

但是很多东西依旧没有成型，揭开angular的面纱的道路好远啊……能把它学完我再回去写react吧