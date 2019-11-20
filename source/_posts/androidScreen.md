---
title: 安卓获取屏幕以及获得像素点
date: 2019-11-19 20:14:38
tags: android
---

# 闲人屁事多

由于一些不可告人的需求，所以开始寻找各种可以实现安卓实时获得屏幕上某个像素点的功能

首先，将需求进行拆解，分别为

1、获得屏幕
2、获得屏幕上一个像素点

# 获得屏幕

获得屏幕分为比较多种的方式，在以前大致分为

+ adb screencap 获取当前屏幕
+ linux 底层 通过frameBuffer获得屏幕信息
我没试过我也不知道好不好用
[参考1](https://blog.csdn.net/kong92917/article/details/50495740)
[参考2](https://blog.csdn.net/u010037124/article/details/38468855)
+ 通过 MediaProjection 屏幕录制获得图像
[参考](https://blog.csdn.net/rrrrrr123rrr/article/details/78884529)

### 我使用的是 MediaProject 的方法，这里重点实现了这个方法

由于我的需求是需要处处调用的，所以将获取像素点的一系列方法封装在一个单例之中，以达到一处声明，处处调用

```kotlin
package com.tools.automator.core

import android.graphics.Bitmap
import android.app.ActivityManager
import android.content.Context
import android.graphics.PixelFormat
import android.hardware.display.DisplayManager
import android.media.Image
import android.media.ImageReader
import android.media.projection.MediaProjection
import android.util.DisplayMetrics
import android.util.Log
import android.view.WindowManager
import java.nio.ByteBuffer

class Screen private constructor(context: Context){

    internal val TAG = "Screen"
    var reader:ImageReader? = null;
    var bitmap:Bitmap? = null;
    var windowManager = context.getSystemService(Context.WINDOW_SERVICE) as WindowManager;

    // 单利
    companion object {
        @Volatile private var INSTANCE: Screen? = null;
        @Volatile private var mediaProjection:MediaProjection? = null;

        fun getInstance(context: Context): Screen =
            INSTANCE?: synchronized(this){
                INSTANCE?: buildScreen(context).also { INSTANCE = it }
            }
        fun getInstance():Screen {
            return INSTANCE!!
        }

        // 初始化函数，用于传入MediaProjection
        fun setMedia(mMediaProjection: MediaProjection){
            mediaProjection=mMediaProjection
        }

        // 创建屏幕
        private fun buildScreen(context: Context) = Screen(context)

    }

    // 获得颜色，RGBColor是一个自己封装的类，用于返回带有RGB值的对象
    fun getPixelColor(x: Int,y: Int):RGBColor{
        return RGBColor(getColor(x,y))
    }

    // 创建虚拟屏幕
    fun setUpVirtualDisplay() {
        var dm:DisplayMetrics = DisplayMetrics();
        windowManager.defaultDisplay.getRealMetrics(dm)

        var imageReader:ImageReader = ImageReader.newInstance(dm.widthPixels, dm.heightPixels, PixelFormat.RGBA_8888, 1);
        // 实时获得屏幕
        mediaProjection?.createVirtualDisplay("ScreenCapture",
            dm.widthPixels, dm.heightPixels, dm.densityDpi,
            DisplayManager.VIRTUAL_DISPLAY_FLAG_AUTO_MIRROR,
            imageReader.getSurface(), null, null);
        reader = imageReader;
    }

    // 使用BitMap获得颜色，并返回
    internal fun getColor(x:Int, y:Int):Int {
        if (reader == null) {
            Log.w(TAG, "getColor: reader is null");
            return -1;
        }

        val image:Image? = reader!!.acquireLatestImage();

        if (image == null) {
            if (bitmap == null) {
                Log.w(TAG, "getColor: image is null");
                return -1;
            }
            return bitmap!!.getPixel(x, y);
        }
        var width:Int = image.getWidth();
        var height:Int = image.getHeight();
        val planes = image.getPlanes();
        val buffer: ByteBuffer = planes[0].getBuffer();
        var pixelStride = planes[0].getPixelStride();
        var rowStride = planes[0].getRowStride();
        var rowPadding = rowStride - pixelStride * width;
        if (bitmap == null) {
            bitmap = Bitmap.createBitmap(width + rowPadding / pixelStride, height, Bitmap.Config.ARGB_8888);
        }
        bitmap!!.copyPixelsFromBuffer(buffer);
        image.close();

        return bitmap!!.getPixel(x, y);
    }
}
```

颜色类
```kotlin
package com.tools.automator.core

import android.graphics.Color

class RGBColor{
    var red:String;
    var green:String;
    var blue:String;
    var alpha:String;
    constructor(color:Int){
        red = Color.red(color).toString(16)
        blue = Color.blue(color).toString(16)
        green = Color.green(color).toString(16)
        alpha = Color.alpha(color).toString(16)
    }

    fun getColor():RGBColor{
        return this
    }
}
```

初始化
```kotlin
    Screen.setMedia(mMediaProjection);
    Screen.getInstance(this).setUpVirtualDisplay();
```
