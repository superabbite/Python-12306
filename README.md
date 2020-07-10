前言

又要过年了，今年你不妨自己写一段代码来抢回家的火车票，是不是很Cool。下面话不多说了，来一起看看详细的介绍吧。

先准备好：

12306网站用户名和密码
chrome浏览器及下载chromedriver
下载Python代码，来自网络整理 [点击下载 |  本地下载 ]

代码用的Python+Splinter开发，Splinter是一个使用Python开发的开源Web应用测试工具，它可以帮你实现自动浏览站点和与其进行交互。

Splinter官网：http://splinter.readthedocs.io/en/latest/。

Splinter执行的时候会自动打开你指定的浏览器，访问指定的URL。然后你所开发的模拟的任何行为，都会自动完成，你只需要坐在电脑面前，像看电影一样看着屏幕上各种动作自动完成然后收集结果即可。

了解原理：

找到相应URL，找到控件模拟登录、查询、订票操作。关键是找到控件名称，难点是起始地不是直接输入的页面值，需要在cookie中查出。

12306查询URL: https://kyfw.12306.cn/otn/leftTicket/init

12306登录URL: https://kyfw.12306.cn/otn/login/init

我的12306URL: https://kyfw.12306.cn/otn/index/initMy12306

购票确认URL: https://kyfw.12306.cn/otn/confirmPassenger/initDc

Python代码打开URL，找到控件填充值：


 def login(self):
  self.driver.visit(self.login_url)
  # 填充用户名
  self.driver.fill("loginUserDTO.user_name", self.username)
  # 填充密码
  self.driver.fill("userDTO.password", self.passwd)
  print u"等待验证码，自行输入..."

找到用户名密码控件名

找到起始地控件名

确定起始地的值，方法Chrome浏览器中的“检查”功能（按F12），Network —> Cookies中找到：

cookie中起始地的值

拷贝起始地的cookie值，我把几个常用的城市拷出来，放到了字典中：


cities= {'成都':'%u6210%u90FD%2CCDW',
'重庆':'%u91CD%u5E86%2CCQW',
'北京':'%u5317%u4EAC%2CBJP',
'广州':'%u5E7F%u5DDE%2CGZQ',
'杭州':'%u676D%u5DDE%2CHZH',
'宜昌':'%u5B9C%u660C%2CYCN',
'郑州':'%u90D1%u5DDE%2CZZF',
'深圳':'%u6DF1%u5733%2CSZQ',
'西安':'%u897F%u5B89%2CXAY',
'大连':'%u5927%u8FDE%2CDLT',
'武汉':'%u6B66%u6C49%2CWHN',
'上海':'%u4E0A%u6D77%2CSHH',
'南京':'%u5357%u4EAC%2CNJH',
'合肥':'%u5408%u80A5%2CHFH'}

查询车票代码：


   print u"购票页面开始..."
   # 加载查询信息
   self.driver.cookies.add({"_jc_save_fromStation": self.starts})


self.driver.cookies.add({"_jc_save_toStation": self.ends})
self.driver.cookies.add({"_jc_save_fromDate": self.dtime})
self.driver.find_by_text(u"查询").click()

其实，你只需要运行代码：


python tickets.py 上海 广州 2018-02-05

当然，还需要手动点一下的还是万恶的12306验证码，抢到票后确认支付就行啦。

抢票进行中

抢票成功！

总结

以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家对软件开发网的支持。