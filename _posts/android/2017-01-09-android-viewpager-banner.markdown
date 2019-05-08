---
layout: post
title:  "Android ViewPager实现的图片轮播"
date:   2017-01-09 11:53:27 +0800
categories: android
---

Android ViewPager实现的简单图片轮播


   HoriziontalBannerView hsv = (HoriziontalBannerView) findViewById(R.id.h_banner);
    hsv.init() ;
 
    ArrayList data = new ArrayList() ;
    data.add(R.drawable.banner_1) ;
    data.add(R.drawable.banner_2) ;
    data.add(R.drawable.banner_3) ;
 
    hsv.setBannerData(data);
 
    hsv.startFlipping();


[源码地址](https://github.com/ngLiaXL/Banner)





