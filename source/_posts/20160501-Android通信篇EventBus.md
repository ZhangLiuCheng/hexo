---
title: Android通信篇EventBus
date: 2016-05-01 21:23:20
tags: 
	- android
categories:
	- App开发
---
{% blockquote %}
<strong>人生如果错了方向，停止就是进步。</strong>
{% endblockquote %}

**EventBus**是一个Android端优化的publish/subscribe消息总线，简化了应用程序内各组件间、组件与后台线程间的通信。比如请求网络，等网络返回时通过Handler或Broadcast通知UI，两个Fragment之间需要通过Listener通信，这些需求都可以通过**EventBus**实现。

作为一个消息总线，有三个主要的元素：
* Event:事件
* Subscriber:事件订阅者，接收特定的事件
* Publisher:事件发布者，用于通知Subscriber有事件发生

### EventBus in 3 step
*** 1.定义Event: ***
{% codeblock lang:java %}
public class MessageEvent { /* Additional fields if needed */ }
{% endcodeblock %}
<!--more-->

*** 2.注册监听: ***
在Activity分别在onCreate和onDestory分别调用下面两方法(或者onStart和onStop,按具体情况调用)
{% codeblock lang:java %}
EventBus.getDefault().register(this);
EventBus.getDefault().unregister(this);
{% endcodeblock %}

在onActivity定义处理方法，方法名字没规定，但一定要在需要处理的上面添加@Subscribe注解，
Subscribe值是ThreadMode.MAIN，根据mode决定方法在什么线程中执行。
{% codeblock lang:java %}
@Subscribe(threadMode = ThreadMode.MAIN)
public void onMessageEvent(MessageEvent event) {}
{% endcodeblock %}

ThreadMode里面有PostThread，MainThread，BackgroundThread和Async.
* ***PostThread***：事件的处理在和事件的发送在相同的进程，所以事件处理时间不应太长，不然影响事件的发送线程，而这个线程可能是UI线程。对应的函数名是onEvent。
* ***MainThread***: 事件的处理会在UI线程中执行。事件处理时间不能太长，这个不用说的，长了会ANR的，对应的函数名是onEventMainThread。
* ***BackgroundThread***：事件的处理会在一个后台线程中执行，对应的函数名是onEventBackgroundThread，虽然名字是BackgroundThread，事件处理是在后台线程，但事件处理时间还是不应该太长，因为如果发送事件的线程是后台线程，会直接执行事件，如果当前线程是UI线程，事件会被加到一个队列中，由一个线程依次处理这些事件，如果某个事件处理时间太长，会阻塞后面的事件的派发或处理。
* ***Async***：事件处理会在单独的线程中执行，主要用于在后台线程中执行耗时操作，每个事件会开启一个线程（有线程池），但最好限制线程的数目。

** 注：如果一个类里面有多个@Subscribe修饰的监听方法，将通过参数来区分调用方法 **

*** 3.发送事件: ***
EventBus.getDefault().post(event)。

### 个人理解
**EventBus** 从功能上来说，和广播比较相似，只是eventBus回调时可以指定处理方法的线程。
适当的运用可以提高代码的灵活性，但如果大量使用，或者代码结构不是很清晰，会使调试变的困难，尤其是团队一起开发，各自技术水平不相同的情况下，使用要谨慎。
**BoradCast** 我一般用法是在A类注册广播，同样在A里面添加静态方法发送广播，这样在别的B类里面发送广播，只需调用A类静态方法，这样既实现A,B之间通信，又便于调试。有时间单独写个文章说明。


<a href="https://github.com/greenrobot/EventBus" target="_blank">EventBus Github地址</a>