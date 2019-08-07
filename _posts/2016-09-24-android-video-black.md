---
layout: post
title:  "[Android]	视频轮播间隙黑屏问题"
date:   2016-09-24 11:53:27 +0800
categories: android
---

Android视频播放通常的做法是MediaPlayer+SurfaceView,在电视上会出现切换视频间隙短暂黑屏现象，修改为MediaPlayer+TextureView，在开启硬件加速的窗口中使用，会解决此问题







