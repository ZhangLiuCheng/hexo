---
title: Android-SlidingPanLayout用法
date: 2015-06-02 15:45:41
tags:
	- SlidingPanLayout
	- 手势滑动返回
categories:
 	- App开发
---
{% blockquote %}
<strong>人生一条路，走自己的路，</strong>
<strong>人生两个好，身体好，心情好</strong>
<strong>好好珍惜每一天！</strong>
{% endblockquote %}


先看效果
!["图片描述"](http://7xsp4m.com1.z0.glb.clouddn.com/Android-SlidingpanLayout.gif)  

### 1.左侧菜单栏
{% codeblock lang:java %}
<android.support.v4.widget.SlidingPaneLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/slidingPaneLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="false"
    tools:context="com.innotect.slidingpanelayoutdemo.GeneralActivity">

    <LinearLayout
        android:layout_width="180dp"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <ListView
            android:id="@+id/listview"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolBar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            android:theme="@style/AppTheme.AppBarOverlay"
            app:layout_scrollFlags="scroll|enterAlways"
            app:navigationIcon="@mipmap/ic_launcher"
            app:title="Slidingpanelayout基本用法"
            app:titleTextColor="#FFFFFF" />

        <WebView
            android:id="@+id/webView"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
    </LinearLayout>
</android.support.v4.widget.SlidingPaneLayout>
{% endcodeblock %}
<!--more-->

### 2.手势滑动返回
{% codeblock lang:java %}
public class BaseSwipeBackActivity extends AppCompatActivity implements SlidingPaneLayout.PanelSlideListener{

    private SlidingPaneLayout mSlidingPaneLayout;

    private FrameLayout mContainer;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        mSlidingPaneLayout = getSlidingPaneLayout(this);
        mSlidingPaneLayout.setPanelSlideListener(this);
        mContainer = getContainerView(mSlidingPaneLayout);
    }

    @Override
    public void setContentView(int id) {
        setContentView(getLayoutInflater().inflate(id, null));
    }

    @Override
    public void setContentView(View v) {
        setContentView(v, new LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT));
    }

    public void setContentView(View v, ViewGroup.LayoutParams params) {
        super.setContentView(mSlidingPaneLayout, params);
        mContainer.removeAllViews();
        mContainer.addView(v, params);
    }

    // 创建SlidingPaneLayout
    private SlidingPaneLayout getSlidingPaneLayout(Context context) {
        SlidingPaneLayout slidingLayout = new SlidingPaneLayout(context);
        try {
            Field overHangSize = SlidingPaneLayout.class.getDeclaredField("mOverhangSize");
            overHangSize.setAccessible(true);
            overHangSize.set(slidingLayout, 0);
            slidingLayout.setSliderFadeColor(Color.TRANSPARENT);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return slidingLayout;
    }

    // 创建FrameLayout,setcontentView添加在该FrameLayout上面
    private FrameLayout getContainerView(SlidingPaneLayout slidingPaneLayout) {
        View leftView = new View(this);
        leftView.setLayoutParams(new LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT));
        slidingPaneLayout.addView(leftView);

        FrameLayout container = new FrameLayout(this);
        container.setBackgroundColor(Color.WHITE);
        container.setLayoutParams(new LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT));
        slidingPaneLayout.addView(container);
        return container;
    }

    @Override
    public void onPanelSlide(View panel, float slideOffset) {

    }

    @Override
    public void onPanelOpened(View panel) {
        finish();
    }

    @Override
    public void onPanelClosed(View panel) {
    }
}
{% endcodeblock %}


<a href="https://github.com/ZhangLiuCheng/SlidingPaneLayoutDemo">Demo</a>

