---
layout: post
title: 自动发送邮件功能——python  
date: 2019-03-31
description: 有些代码运行耗时很长，不确定什么时候运行完毕，但是又不能坐在电脑旁一直等它结束。要是代码运行完毕或者报错的时候给自己发送一封邮件多好，这样就可以及时返回进行修改代码，或是进行后续工作。
categories: Python
tags: Python 
author: NMt
mathjax: true
---
* content
{:toc}
有些代码运行耗时很长，不确定什么时候运行完毕，但是又不能坐在电脑旁一直等它结束。要是代码运行完毕或者报错的时候给自己发送一封邮件多好，这样就可以及时返回进行修改代码，或是进行后续工作。  

<div style='display: none'>
@@@@
</div>





{% raw %}
最近又被分到爬虫任务了，平常写写爬虫随便玩下挺好，只针对那种一会就能爬下来的任务，最讨厌的就是那种需要爬上万页的。因为这种爬虫一般耗时很长，隔一段时间IP很有可能被封掉，因此要断点继续，由于网页的页数很多，因此上面的数据格式肯定不会是完全相同，从而在处理数据和存储数据的时候有很大几率会报错。因此很有可能出现的情况就是，满意的看着自己的代码，本以为会万无一失，自信满满的点下运行键，乖乖坐在电脑前盯着程序发现没有出bug，于是安心的去潇洒去了。等估摸着时间差不多了，过去一看，发现程序可能在自己刚转身的时候就报错了。  

  

于是乎，我一怒之下就写了个自动发邮件的程序，原理好像并不难，把代码封装一下就成为自己专属发邮件的小程序，很是方便！  

废话不多说了。接下来就开始吧~  

1. 准备工作：需要两个邮箱号，我用的是两个QQ邮箱。  
2. 授权发送邮件的QQ邮箱。  
        
    首先登陆自己要发送的；邮箱账号，点设置。  
	![][pt_01]  
     
    接下来，点击账户。  
    ![][pt_02]
     
    划到这个界面之后把 `IMAP/SMAP服务`打开，这时候它会让你发送邮件确认。  
    ![][pt_03]  
     
   确认之后它会给一个授权码，下面会用到，复制下来即可。  
    ![][pt_04]
   
3. 前面工作做好了，就把下面的代码复制到自己的py文件中，记得改掉发件人和收件人的邮箱、授权码、发送内容等信息。  
	```python
	# send_email.py
	from email.header import Header
	from email.mime.text import MIMEText
	from email.utils import parseaddr,formataddr
	import smtplib

	def _format_addr(s):    
		name,addr = parseaddr(s)    
		return formataddr((Header(name,'utf-8').encode(),addr))

	#发件人地址
	from_addr = '发送人邮箱@qq.com'
	#密码刚才复制的邮箱的授权码
	password = 'xxxx填写授权码'

	#收件人地址

	to_addr =  '接收者邮箱@qq.com'

	#邮箱服务器地址

	smtp_server = 'smtp.qq.com'

	#设置邮件信息

	msg = MIMEText('xxxxx自己输入','utf-8')

	msg['From'] = _format_addr('发件人名字<%s>'%from_addr)

	msg['To'] = _format_addr('收件人名字<%s>'%to_addr)

	msg['Subject'] = Header('标题','utf-8').encode()

	#发送邮件

	server = smtplib.SMTP_SSL(smtp_server,465)

	#打印出和SMTP服务器交互的所有信息

	server.set_debuglevel(1)

	#登录SMTP服务器

	server.login(from_addr,password)

	#sendmail():发送邮件，由于可以一次发给多个人，所以传入一个list邮件正文是一个str，as_string()把MIMEText对象变成str。

	server.sendmail(from_addr,to_addr,msg.as_string())

	server.quit()

	print('邮件发送成功！')
	```

4. 直接运行整个文件就可以收到发送的信息，这一步是教你如何嵌入到代码中。  

	```python 
	import os
	try:
		list1 = []
		bug = list1[0] # 这里有一个bug
	except:
		os.system('python filename.py') # 我的文件名字改为了send_email.py，所以这里可以写为 os.system('python send_email.py')
	```

参考文献：  

https://jingyan.baidu.com/article/39810a23bc8ad8b636fda6a0.html

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/03/31/autosend_email/)   

<!--以下是本文用到的链接-->
[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/11_autosend_email/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/11_autosend_email/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/11_autosend_email/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/11_autosend_email/04.png

{% endraw %}
