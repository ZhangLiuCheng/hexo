---
title: Android—CoordinatorLayout用法
date: 2015-05-20 17:05:22
tags:
	- CoordinatorLayout
categories:
 	- App开发
---
{% blockquote %}
<strong>人生一条路，走自己的路，</strong>
<strong>人生两个好，身体好，心情好</strong>
<strong>好好珍惜每一天！</strong>
{% endblockquote %}

### 1.
先看效果
!["图片描述"](http://7xsp4m.com1.z0.glb.clouddn.com/Android-CoordinatorLayout.gif)  

{% codeblock lang:java %}
<?xml version="1.0" encoding="utf-8"?>"
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context="com.innotect.coordinatorlayoutdemo.ScrollingActivity">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/appbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/AppTheme.AppBarOverlay">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            app:layout_scrollFlags="scroll|enterAlways"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            app:title="与Scrollview" />
    </android.support.design.widget.AppBarLayout>


    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_margin="@dimen/text_margin"
            android:text="@string/large_text" />

    </android.support.v4.widget.NestedScrollView>


</android.support.design.widget.CoordinatorLayout>
{% endcodeblock %}
<!--more-->

### 2.


<a href="https://github.com/ZhangLiuCheng/CoordinatorLayoutDemo">Demo</a>
