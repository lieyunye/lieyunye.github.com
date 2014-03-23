---
layout: post
title: "在IOS应用中使用字体图标"
date: 2013-10-13 22:39
comments: true
categories: 
---
做IOS开发的都知道，因为屏幕分辨率的问题，在ios app 中都得放两套切图来支持retina屏和非retina屏幕，但是文字就不需要考虑分辨率的问题，所以可不可以将一些图片用文字来代替呢，省时省力省资源。看下面一些截图：

   {% img/images/font-icon-4.png %}
   
   {% img/images/font-icon-5.png %}

   {% img/images/font-icon-6.png %}

   {% img/images/font-icon-7.png %}

   {% img/images/font-icon-8.png %}

   {% img/images/font-icon-9.png %}

   {% img/images/font-icon-10.png %}

   {% img/images/font-icon-11.png %}


这些截图上面的图标都是用文字来表示的，没有用png图片，看起来还不错吧

下面介绍一下制作以及使用图标字体的方法

#字体图标的制作

这部分分两个步骤：1、字形图标的制作 2、字体库的制作

###一、字形图标的制作

   安装一个工具Illustrator，这个工具有破解版，自行搜寻下载
   
   使用其中的钢笔工具绘制字形图标，比如绘制一个新浪微博logo和垃圾桶，如图：
   
   新浪微博logo
   
   {% img/images/font-icon-2.png %}
   
   垃圾桶 
   
   {% img/images/font-icon-3.png %}
   
   
   好了，绘制完字形图标，接下来做字体库
   
###二、字体库的制作

   安装FontLab Studio，这个工具可以生成字体库
   new-->generate font,生成.ttf文件，打开该ttf文件，选中一个字形，打开，然后将做好的字形图标从Illustrator中copy过来，这里有个尺寸问题，可以参看这篇文章[Illustrator+FontLab进行字体设计教程](http://www.webjx.com/illustrator/ai-20782_3.html)
   保存，ok。
   
#字体图标的使用

将制作好的字体库copy到工程中，在IOS工程中配置一下，打开appName-Info.plist,添加属性Fonts provided by application，如图：

{% img/images/font-icon-1.png %}

因为字体图标也是字体，所以使用和普通的字体没区别：看代码就知道了


    StrokeLabel *cameraLabel = [[StrokeLabel alloc] initWithFrame:CGRectMake(175, headerImageView.frame.origin.y + 70, 54, 46) LineWidth:5 TextColor:[UIColor whiteColor]];
    cameraLabel.text = @"C";
    cameraLabel.textAlignment = NSTextAlignmentCenter;
    cameraLabel.textColor= [UIColor colorWithHexString:@"#8dc63f"];
    cameraLabel.backgroundColor = [UIColor clearColor];
    cameraLabel.font = [UIFont fontWithName:FONT_99FANGICON size:35];
    [scrollView addSubview:cameraLabel];
    [cameraLabel release];


这段代码展示一个相机图标

   {% img/images/font-icon-12.png %}

[Demo](https://github.com/lieyunye/font-icon)

大家有兴趣的话，可以在项目中使用，IOS项目和Android项目都可以运用，十分方便，到此为止吧




