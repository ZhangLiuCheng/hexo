---
title: SlideMenu和UIScrollView手势冲突
date: 2016-04-08 16:53:38
tags: 
	- ios
categories:
	- App开发
---
定义一个子类继承UIScrollView，重写`- (BOOL)gestureRecognizerShouldBegin:(UIGestureRecognizer *)gestureRecognizer`方法，做些判断就可以达达这效果。
目前我只做了向右滑动，菜单在左侧的冲突，右边依此类推很容易解决。以下Menu在左侧的解决方法：
<!--more-->
{% codeblock lang:objc %}
- (BOOL)gestureRecognizerShouldBegin:(UIGestureRecognizer *)gestureRecognizer {
    if (![gestureRecognizer isKindOfClass:[UIPanGestureRecognizer class]]) {
        return YES;
    }
    CGPoint velocity = [(UIPanGestureRecognizer *)gestureRecognizer velocityInView:self];
    if (velocity.x > 0.0f && self.contentOffset.x == 0 ) {
        return NO;
    }
    return YES;
}
{% endcodeblock %}
侧边栏我用的是APLSlideMenu。

{% blockquote %}
<strong>不闻不问不一定是忘记了，但一定是疏远了，彼此沉默太久就连主动的都需要勇气。</strong>
{% endblockquote %}