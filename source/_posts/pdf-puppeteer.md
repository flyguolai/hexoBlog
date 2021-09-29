---
title: puppeteer生成pdf
date: 2021-09-29 18:19:38
tags: nodejs pdf
---

# 通过nodejs生成pdf

本质上是通过起一个无头浏览器，通过nodejs进行调用

采用puppeteer进行api的操作

puppeteer的官方demo

```javascript
    const puppeteer = require('puppeteer');

    (async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    await page.goto('https://news.ycombinator.com', {waitUntil: 'networkidle2'});
    await page.pdf({path: 'hn.pdf', format: 'A4'});

    await browser.close();
    })();
```

会自动读取对应的图片与数据

详细的官方文档

https://zhaoqize.github.io/puppeteer-api-zh_CN/#/


## 核心点

### 1、背景色打印不出来

```javascript
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    await page.pdf({
        printBackground:true
    })
```

### 2、文件操作
采用fs-extra进行操作，api方便比fs好用那么一丢丢

```javascript
    const fse = require('fs-extra')
    fse.copy(......)
```

### 3、生成可执行文件

采用库`pkg`

路径采用process.cwd()，不然打包后无法写文件

### 4、pkg打包chromium

```javascript
const path = require('path');
const puppeteer = require('puppeteer');

const isPkg = typeof process.pkg !== 'undefined';

//mac path replace
let chromiumExecutablePath = (isPkg ?
  puppeteer.executablePath().replace(
    /^.*?\/node_modules\/puppeteer\/\.local-chromium/,
    path.join(path.dirname(process.execPath), 'chromium')
  ) :
  puppeteer.executablePath()
);

console.log(process.platform)
//check win32
if (process.platform == 'win32') {
  chromiumExecutablePath = (isPkg ?
    puppeteer.executablePath().replace(
      /^.*?\\node_modules\\puppeteer\\\.local-chromium/,
      path.join(path.dirname(process.execPath), 'chromium')
    ) :
    puppeteer.executablePath()
  );
}

(async () => {

  const browser = await puppeteer.launch({
    executablePath: chromiumExecutablePath,
    headless: false
  });

  const page = await browser.newPage()

  await page.goto('https://www.google.com')

})()
```