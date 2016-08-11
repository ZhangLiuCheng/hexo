---
title: Javapoet生成java文件
date: 2016-04-22 17:00:03
comments: true
tags: 
	- javapoet
categories:
	- java
---

{% blockquote %}
<strong>如果你简单，这个世界就对你简单。</strong>
{% endblockquote %}

官方说明JavaPoet is a Java API for generating .java source files.

原由是看到了<a href="https://github.com/JakeWharton/butterknife" target="_blank">butterknife</a>框架，里面用的是RetentionPolicy.CLASS，编译的时候通过注解实现框架的功能，比RetentionPolicy.RUNNTIME再通过反射实现效率高。
然后就去看RetentionPolicy.CLASS的实现原理，需要注解处理器(Annotation Processor)，最后就一步步的看到了javapoet，自动生成java代码。
扯远了，直接记录过程吧。

### 一.过程搭建：
我是通过eclipse maven自动下载jar的。
{% codeblock lang:java %}
public static void main(String[] args) throws IOException {
	MethodSpec main = MethodSpec
			.methodBuilder("main")
			.addModifiers(Modifier.PUBLIC, Modifier.STATIC)
			.returns(void.class)
			.addParameter(String[].class, "args")
			.addStatement("$T.out.println($S)", System.class, "Hello, JavaPoet!").build();

	TypeSpec helloWorld = TypeSpec.classBuilder("HelloWorld")
			.addModifiers(Modifier.PUBLIC, Modifier.FINAL).addMethod(main)
			.build();

	JavaFile javaFile = JavaFile.builder("com.zlc.javapoet",
			helloWorld).build();
	// 写入src目录下面
	String path = System.getProperty("user.dir") + "/src";
	javaFile.writeTo(new File(path));
}
{% endcodeblock %}
<!--more-->

开始运行，出现了Unsupported major.minor version 51.0错误，网上查了下是jdk版本不一致啥的。我看了下我eclpse引用的jre是mac系统自带的，改成了下载jdk，错误搞定。
运行成功，刷新下，会看到自动生成的HelloWorld.java类。

### 一.更多用法：
{% codeblock lang:java %}
MethodSpec main = MethodSpec.methodBuilder("main")
    .addCode(""
        + "int total = 0;\n"
        + "for (int i = 0; i < 10; i++) {\n"
        + "  total += i;\n"
        + "}\n")
    .build();

// 生成的java方法如下
void main() {
  int total = 0;
  for (int i = 0; i < 10; i++) {
    total += i;
  }
}
{% endcodeblock %}

还有很多功能，参照官方用法，用到的时候再去看吧。
<a href="https://github.com/square/javapoet" target="_blank">github地址</a>



