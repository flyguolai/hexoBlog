---
title: Angular在懒加载子模块无法触发主模块的interceptor
date: 2020-09-16 11:33:10
tags: interceptor
---

# 问题

书写了一个service，请求后端，但是无法身份验证拦截，没有将其返回登陆界面

# 调查

通过log的方式检查，发现其本身无法被触发，在intercept钩子中无法触发响应，但是也有其他模块的http service是可以正常访问的

# 发现与猜测

通过代码比对与逻辑思考，发现我当初企图将service挂载在子模块上的行为导致了这个结果(将providedIn改为了any)

而interceptor是挂载在app层的

将providedIn修改为root即可修复该问题

在网上进行搜索的时候发现大部分人也没有修改过providedIn这个DI配置项。。。也就没啥找到可以参考的案例

猜测是模块隔离导致的无法访问，暂且先搁置

先记录下来，以便之后review
