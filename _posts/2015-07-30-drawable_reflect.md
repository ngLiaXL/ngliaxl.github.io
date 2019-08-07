---
layout: post
title:  "[Android]	反射drawable"
date:   2015-07-30 11:53:27 +0800
categories: android
---



	private int getDrawableByReflect(String field) {
		int drawable = 0;
		// "cn.testreflect.R$drawable"
		String pkg = MainApplication.getContext().getPackageName();
		String clazz = pkg + ".R$drawable";
		try {
			Field f = Class.forName(clazz).getField(field);
			drawable = f.getInt(field);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return drawable;
	}





