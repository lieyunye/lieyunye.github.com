---
layout: post
title: "搭建一个网页抓取服务"
date: 2014-02-18 11:50
comments: true
categories: 
---
这是一个基于python的网页抓取服务，主要用到两个第三方库，即PhantomJS和Selenium，当然具体解析网页还需要别的库，之后用到再说。

[PhantomJS](http://phantomjs.org/):是一个基于WebKit的服务器端 JavaScript API。它全面支持web而不需浏览器支持，其快速，原生支持各种Web标准： DOM 处理, CSS 选择器, JSON, Canvas, 和 SVG。PhantomJS可以用于页面自动化，网络监测，网页截屏，以及无界面测试等。

[Selenium](http://docs.seleniumhq.org/):是一个自动化web应用程序测试工具,在这里我们使用的是[Python的一个模块](https://pypi.python.org/pypi/selenium)

我所使用的系统是centos6.5，已安装好python环境，接下来需要安装[django](https://www.djangoproject.com/):一个开放源代码的Web应用框架，由Python写成,采用了MVC的软件设计模式.

####安装Django

在安装Django之前需要安装两个小工具easy_install和virtualenv

1、easy_install和pip：很多情况下我们都需要给python安装很多的第三方扩展包，安装这些包有点小繁琐，而easy_install和pip可以一键安装，方便又容易。

	* sudo python setup.py install
	* which easy_install
	* #/usr/bin/easy_install
	* sudo easy_install pip

2、virtualenv和virtualenvwrapper：一种源代码版本管理工具，可以使每个project都有自己独立的虚拟环境。

	* sudo easy_install virtualenv virtualenvwrapper
	
安装好[virtualenv](http://www.virtualenv.org/en/latest/index.html)和[virtualenvwrapper](http://virtualenvwrapper.readthedocs.org/en/latest/command_ref.html)之后，需要配置一下环境，在home目录下打开.bashrc
	
	if [ -f /usr/local/bin/virtualenvwrapper.sh]; then   export VENVWRAP=/usr/local/bin/virtualenvwrapper.sh
	fi
	if [ ! -z $VENVWRAP]; then   [ -d $HOME/sites/env ] || mkdir -p $HOME/sites/env
          export WORKON_HOME=$HOME/sites/env
          source $VENVWRAP
          export VIRTUALENV_USE_DISTRIBUTE=true
          export PIP_VIRTUALENV_BASE=$WORKON_HOME
          export PIP_REQUIRE_VIRTUALENV=true
          export PIP_RESPECT_VIRTUALENV=true
          export PIP_DOWNLOAD_CACHE=$HOME/.pip/cache
	fi


将以上代码加入.bashrc，然后source .bashrc

以下是在安装过程中可能会遇到的一些问题
	
如果遇到：

	Traceback (most recent call last):
  		File "/usr/bin/easy_install", line 5, in <module>
    	from pkg_resources import load_entry_point
  		File "build/bdist.linux-x86_64/egg/pkg_resources.py", line 2720, in <module>
  		File "build/bdist.linux-x86_64/egg/pkg_resources.py", line 588, in resolve
		pkg_resources.DistributionNotFound: setuptools==3.0dev
		
解决方法：
	
	wget http://python-distribute.org/distribute_setup.py
	sudo python distribute_setup.py 

如果遇到
	
	
	Traceback (most recent call last):
  	File "distribute_setup.py", line 556, in <module>
    sys.exit(main())
  	File "distribute_setup.py", line 552, in main
    tarball = download_setuptools(download_base=options.download_base)
  	File "distribute_setup.py", line 211, in download_setuptools
    src = urlopen(url)
  	File "/usr/local/lib/python2.7/urllib2.py", line 127, in urlopen
    return _opener.open(url, data, timeout)
  	File "/usr/local/lib/python2.7/urllib2.py", line 410, in open
    response = meth(req, response)
  	File "/usr/local/lib/python2.7/urllib2.py", line 523, in http_response
    'http', request, response, code, msg, hdrs)
  	File "/usr/local/lib/python2.7/urllib2.py", line 442, in error
    result = self._call_chain(*args)
  	File "/usr/local/lib/python2.7/urllib2.py", line 382, in _call_chain
    result = func(*args)
  	File "/usr/local/lib/python2.7/urllib2.py", line 629, in http_error_302
    return self.parent.open(new, timeout=req.timeout)
  	File "/usr/local/lib/python2.7/urllib2.py", line 404, in open
    response = self._open(req, data)
  	File "/usr/local/lib/python2.7/urllib2.py", line 427, in _open
    'unknown_open', req)
  	File "/usr/local/lib/python2.7/urllib2.py", line 382, in _call_chain
    result = func(*args)
  	File "/usr/local/lib/python2.7/urllib2.py", line 1247, in unknown_open
    raise URLError('unknown url type: %s' % type)
	urllib2.URLError: <urlopen error unknown url type: https>
	
解决办法：

	sudo yum install openssl-devel


好了，先创建一个虚拟环境

	mkvirtualenv yichangxi
	
创建一个requirements.txt，内容是django

[安装Django](https://www.djangoproject.com/download/)

	pip install -r requirements.txt


访问localhost:8080就可以看到Django的欢迎页了。

因为我是用vagrant在本机（mac）虚拟的centos，所以为了可以使本地与虚拟的系统中的Django服务进行通信，所以安装了[Tengine](http://tengine.taobao.org/document_cn/install_cn.html).OK

接下来我们需要安装这几个第三方模块，simplepython,BeautifulSoup,selenium,pylibmc.

如果在安装pylibmc时遇到问题：

		Command /home/vagrant/sites/env/xxx/bin/python -c "import setuptools, tokenize;__file__='/home/vagrant/sites/env/xxx/build/pylibmc/setup.py';exec(compile(getattr(tokenize, 'open', open)(__file__).read().replace('\r\n', '\n'), __file__, 'exec'))" install --record /tmp/pip-DvN1EK-record/install-record.txt --single-version-externally-managed --compile --install-headers /home/vagrant/sites/env/xxx/include/site/python2.7 failed with error code 1 in /home/vagrant/sites/env/xxx/build/pylibmc
	Storing debug log for failure in /home/vagrant/.pip/pip.log

解决办法：
	
	sudo yum install libmemcached-devel zlib1g-devel libssl-devel python-devel build-essential
	
如果遇到：

		RuntimeError: pylibmc requires >= libmemcached 0.32, was compiled with 0.31

解决办法：

		cd /usr/local/lib/
		ln -s libmemcached.so libmemcached.so.11
		sudo ldconfig
		ldconfig -p | grep libmemcached查看一下
		安装libmemcached 0.32，重新安装pylibmc


如果遇到：

		django.core.exceptions.ImproperlyConfigured: Error loading either pysqlite2 or sqlite3 modules (tried in that order): No module named _sqlite3
		
解决办法：

	sudo yum install pysqlite

如果遇到：
	
	/usr/bin/yum: line 2: import: command not found
	/usr/bin/yum: line 3: try:: command not found
	/usr/bin/yum: line 4: import: command not found
	/usr/bin/yum: line 5: except: command not found
	/usr/bin/yum: line 23: syntax error near unexpected token `('
	/usr/bin/yum: line 23: `""" % (sys.exc_value, sys.version)'

解决办法：

	vi /usr/bin/yum
	第一行修改为 ：#!/usr/bin/python2.6
	
如果遇到：

	pip install pylibmc
	Downloading/unpacking pylibmc
  	Downloading pylibmc-1.3.0.tar.gz (49kB): 49kB downloaded
  	Storing download in cache at /home/op/.pip/cache/https%3A%2F%2Fpypi.python.org	%2Fpackages%2Fsource%2Fp%2Fpylibmc%2Fpylibmc-1.3.0.tar.gz
  	Running setup.py (path:/home/op/pyServer/sites/env/yichangxi/build/pylibmc/	setup.py) egg_info for package pylibmc
    
    warning: no files found matching 'LICENSE'
    warning: no files found matching 'runtests.py'
    warning: no files found matching '*.py' under directory 'pylibmc'
	Installing collected packages: pylibmc
  	Running setup.py install for pylibmc
    building '_pylibmc' extension
    gcc -pthread -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -	Wstrict-prototypes -fPIC -DUSE_ZLIB -I/usr/local/include/python2.7 -c src/	_pylibmcmodule.c -o build/temp.linux-x86_64-2.7/src/_pylibmcmodule.o -fno-	strict-aliasing
    In file included from src/_pylibmcmodule.c:34:
    src/_pylibmcmodule.h:189: error: ‘MEMCACHED_BEHAVIOR_TCP_KEEPALIVE’ undeclared 	here (not in a function)
    src/_pylibmcmodule.h:256: error: ‘MEMCACHED_DISTRIBUTION_CONSISTENT_KETAMA_SPY’ 	undeclared here (not in a function)
    src/_pylibmcmodule.h:256: error: initializer element is not constant
    src/_pylibmcmodule.h:256: error: (near initialization for 	‘PylibMC_distributions[3].flag’)
    src/_pylibmcmodule.h:261: error: ‘MEMCACHED_DISTRIBUTION_CONSISTENT_MAX’ 	undeclared here (not in a function)
    src/_pylibmcmodule.h:261: error: initializer element is not constant
    src/_pylibmcmodule.h:261: error: (near initialization for 	‘PylibMC_distributions[5].flag’)
    error: command 'gcc' failed with exit status 1
    Complete output from command /home/op/pyServer/sites/env/yichangxi/bin/python -	c "import setuptools, tokenize;__file__='/home/op/pyServer/sites/env/yichangxi/	build/pylibmc/setup.py';exec(compile(getattr(tokenize, 'open', open)	(__file__).read().replace('\r\n', '\n'), __file__, 'exec'))" install --record /	tmp/pip-0zWJqI-record/install-record.txt --single-version-externally-managed --	compile --install-headers /home/op/pyServer/sites/env/yichangxi/include/site/	python2.7:
    running install

	running build

	running build_py

	creating build

	creating build/lib.linux-x86_64-2.7

	creating build/lib.linux-x86_64-2.7/pylibmc

	copying src/pylibmc/test.py -> build/lib.linux-x86_64-2.7/pylibmc

	copying src/pylibmc/consts.py -> build/lib.linux-x86_64-2.7/pylibmc

	copying src/pylibmc/__main__.py -> build/lib.linux-x86_64-2.7/pylibmc

	copying src/pylibmc/pools.py -> build/lib.linux-x86_64-2.7/pylibmc

	copying src/pylibmc/__init__.py -> build/lib.linux-x86_64-2.7/pylibmc

	copying src/pylibmc/client.py -> build/lib.linux-x86_64-2.7/pylibmc

	running build_ext

	building '_pylibmc' extension

	creating build/temp.linux-x86_64-2.7

	creating build/temp.linux-x86_64-2.7/src

	gcc -pthread -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -DUSE_ZLIB -I/usr/local/include/python2.7 -c src/_pylibmcmodule.c -o build/temp.linux-x86_64-2.7/src/_pylibmcmodule.o -fno-strict-aliasing

	In file included from src/_pylibmcmodule.c:34:

	src/_pylibmcmodule.h:189: error: ‘MEMCACHED_BEHAVIOR_TCP_KEEPALIVE’ undeclared here (not in a function)

	src/_pylibmcmodule.h:256: error: ‘MEMCACHED_DISTRIBUTION_CONSISTENT_KETAMA_SPY’ undeclared here (not in a function)

	src/_pylibmcmodule.h:256: error: initializer element is not constant

	src/_pylibmcmodule.h:256: error: (near initialization for ‘PylibMC_distributions[3].flag’)

	src/_pylibmcmodule.h:261: error: ‘MEMCACHED_DISTRIBUTION_CONSISTENT_MAX’ undeclared here (not in a function)

	src/_pylibmcmodule.h:261: error: initializer element is not constant

	src/_pylibmcmodule.h:261: error: (near initialization for ‘PylibMC_distributions[5].flag’)

	error: command 'gcc' failed with exit status 1

解决办法：

	这个错误的出现是因为libmemcached的版本与pylibmc的版本不匹配造成的，所以安装相对应的libmemcached与pylibmc即可
	
未完待续~


