---
layout: post
title: "解决IOS状态栏高度变化对View的影响"
date: 2014-03-23 16:02
comments: true
categories: 
---
在IOS应用中会遇到状态栏高度的变化，比如手机来电话和手机设置热点。
这时statusBarFrame.size.height=40;并且self.view回整体向下偏移20的高度，如果程序界面中的底部有一些视图是根据self.view.frame.size.height来设置位置的，那么就会被遮挡，这真悲剧。

UIApplicationWillChangeStatusBarFrameNotification是用来通知状态栏的改变，据此便可以相应调整视图的位置

如果在statusBarFrame变化之前对self.view的transform做了改变，那么此时self.view.frame不再是（0，0，320，568）
因为view.frame是根据view.bounds和view.center来确定的，所以可以由此来重新调整self.view.frame

OK，做个标记，到此为止吧
