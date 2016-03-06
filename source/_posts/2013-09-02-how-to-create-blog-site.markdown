---
layout: post
title: "本博客的搭建"
date: 2013-09-02 19:44
comments: true
categories: summary
---
如果有人对git-hub + octopress的博客感兴趣的话，可以读一读下文：

1、首先读一下以下三个链接的文章

  * [象写程序一样写博客：搭建基于github的博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/) @唐巧
  * [利用Octopress搭建一个Github博客](http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/) @破船(BeyondVincent)
  * [在github上使用octopress](http://huanggang.me/archives/654)
 
2、顺利的话，相信你已经搭建成功了，如果出了问题，可以看看下文，梳理一下：

  * git  
  
  	将git安装好，有git-hub的账号,对git做一些基本配置，比如:remote.origin.url = https://github.com/username/username.github.com.git ， 这样git push origin source 才不会出错
  	
  * 安装ruby和rvm 
  
  	记得安装好相关依赖项，如 bundle
  	
  * 安装octopress
	
	生成博客或者是对octopress设置 都要记得 cd your_local_octopress_directory
	
  	
##其他配置

  * 添加音频
  
  		下载[audio.js](http://kolber.github.io/audiojs/)
  		将其中的audio.min.js和player-graphics.gif拷贝到/source/javascripts目录下
  		编辑/source/_includes/head.html,加入如下代码：
  		<script src="{{ root_url }}/javascripts/audio.min.js"></script>
 		<script>
   			audiojs.events.ready(function() {
     		var as = audiojs.createAll();
   			});
  		</script>
  		下载[Audio/MP3 Plugin for Octopress](https://gist.github.com/lieyunye/409b6c76f5d9dc4d7a5b)将其放在/plugins目录下
  		最后编辑xxx.markdown 加入audio标签
  		
  
  * 导航栏的配置
  
  		cd source/_includes/custom
  		<li><a href="{{ root_url }}/">首页</a></li>
  		<li><a href="{{ root_url }}/blog/archives">所有文章</a></li>
  		<li><a href="http://weibo.com/lieyunye" target="_blank">我的微博</a></li>
  		
  * 关于我
  	
  		cd octopress/source/_includes/custom/asides
  		<h1>About Me</h1>
  		<p>爱帮网(2011---2012)<br/>
     	   九九房(2012---至今)</p>
        <br/>Java 开发工程师，曾开发爱帮生活、爱帮公交后台
        <br/>IOS 开发工程师，曾开发爱帮生活、掌上租房、找室友
        <br/>
        <br/>新浪微博：<a href='http://weibo.com/lieyunye' target='_blank'>裂云>野</a>
		</p>
  	
  	参见文档：http://octopress.org/docs/theme/template
  	http://micronarrativ.org/blog/blog/2012/09/06/wordpress-til-octopress-audio/
  
  
  OK,这就是本博客的搭建过程，到此为止吧