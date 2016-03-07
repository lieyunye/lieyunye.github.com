---
layout: post
title: "创建Tweak时遇到的问题及其解决方法"
date: 2014-04-02 13:26
comments: true
categories: 
---
问题1：

	dpkg: status database area is locked by another process
	make: *** [internal-install] Error 2

解决办法：
	
	重启SpringBoard即可

问题2：

	Undefined symbols for architecture armv7:
  	"_OBJC_CLASS_$_UIAlertView", referenced from:
      objc-class-ref in Tweak.xm.e98a028e.o
	ld: symbol(s) not found for architecture armv7
	clang: error: linker command failed with exit code 1 (use -v to see invocation)
	make[2]: *** [obj/iOSRE.dylib.ba964c90.unsigned] Error 1
	make[1]: *** [internal-library-all_] Error 2
	make: *** [iOSRE.all.tweak.variables] Error 2
	
解决方法：

	在MakeFile中加入  xxx_FRAMEWORKS = UIKit
	将xxx替换成你的TWEAK_NAME
	
问题3：

	iosre/theos/makefiles/targets/Darwin/iphone.mk:41: Deploying to iOS 3.0 while building for 6.0 will generate armv7-only binaries.

解决方法：

	在MakeFile中加入 ARCHS = armv7 armv7s
	
以上就是一些搜集到的解决方法，仅供参考，到此为止吧

参考链接：[Important: Update your tweaks to support arm64](http://sharedinstance.net/2013/12/how-to-support-arm64/)

	
	