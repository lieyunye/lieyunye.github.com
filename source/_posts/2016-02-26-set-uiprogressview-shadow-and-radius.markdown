---
layout: post
title: "UIProgressView设置shadow和radius"
date: 2016-02-26 17:22
comments: true
categories: 
---

时间：2016-02-26

地点：公司

人物：lieyunye

起因：遇到一个需求，需要给进度条加个投影，并且进度条的首尾要有圆角。

经过：项目中用到的是UIProgressView

1、用layer来做
	
	 _videoProgressView.trackTintColor = [UIColor colorWithRed:1 green:1 blue:1 alpha:0.18];
    _videoProgressView.progressTintColor = [UIColor whiteColor];
    _videoProgressView.layer.cornerRadius = 1.5;
    _videoProgressView.layer.masksToBounds = YES;
    _videoProgressView.layer.shadowOpacity = 0.5;
    _videoProgressView.layer.shadowColor = [UIColor blackColor].CGColor;
    _videoProgressView.layer.shadowOffset = CGSizeMake(1, 1);
        
结果就是有圆角但是没有投影的效果，原因是masksToBounds=YES将投影效果取消了。

2、用加投影的图片

	[_videoProgressView setProgressImage:[UIImage imageNamed:@"videoProgressTrack1"]];
    [_videoProgressView setTrackImage:[UIImage imageNamed:@"videoProgressTrack2"]];
    _videoProgressView.layer.shadowOpacity = 0.5;
    _videoProgressView.layer.shadowColor = [UIColor blackColor].CGColor;
    _videoProgressView.layer.shadowOffset = CGSizeMake(1, 1);
        
 很抱歉，iOS8到iOS9系统上设置图片是没有效果的，要么是个蓝色条，要么什么都没有，也google了不少，貌似是UIProgressView的BUG
 
 那么打印一下_videoProgressView看看他的subview都是什么情况
 
 	<__NSArrayM 0x17d07ed0>(
	<UIImageView: 0x17fe5d30; frame = (0 0; 168 2); opaque = NO; 	userInteractionEnabled = NO; layer = <CALayer: 0x17d28700>>,
	<UIImageView: 0x17fdd160; frame = (0 0; 0 2); opaque = NO; 	userInteractionEnabled = NO; layer = <CALayer: 0x17dcfc70>>
	)
 
 其中一个UIImageView的width是0！在iOS7.0.x上progressImage有时会突然消失，而这时打印上面的imageView的image为nil
 
 	UIImageView *trackImageView = _videoProgressView.subviews.firstObject;
    UIImageView *progressImageView = _videoProgressView.subviews.lastObject;
    CGRect trackProgressFrame = trackImageView.frame;
    trackProgressFrame.size.height = _videoProgressView.frame.size.height;
    trackImageView.frame = trackProgressFrame;
    progressImageView.frame = trackProgressFrame;
    progressImageView.image = progressImage;
    trackImageView.image = trackImage;
 那么如果将UIImageView2的width设置为UIImageView1的width，那么在iOS8和iOS9上设置图片就OK了，只是iOS7上就不好使了

结果：

最终的解决办法：

	_videoProgressView.tintColor = [UIColor clearColor];
    _videoProgressView.trackTintColor = [UIColor colorWithRed:1 green:1 blue:1 alpha:0.18];
    _videoProgressView.progressTintColor = [UIColor whiteColor];
    UIImageView *trackImageView = _videoProgressView.subviews.firstObject;
    UIImageView *progressImageView = _videoProgressView.subviews.lastObject;
    CGRect trackProgressFrame = trackImageView.frame;
    trackProgressFrame.size.height = _videoProgressView.frame.size.height;
    trackImageView.frame = trackProgressFrame;
    progressImageView.frame = trackProgressFrame;
    trackImageView.layer.cornerRadius = 1.5;
    trackImageView.layer.masksToBounds = YES;
    progressImageView.layer.cornerRadius = 1.5;
    progressImageView.layer.masksToBounds = YES;
    
 总之系统上如果有一个问题，那么就会折腾你好长好长时间，虽然这只是一个小小的需求...
 
 到此为止吧