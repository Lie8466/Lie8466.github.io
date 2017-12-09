---
title: Charles使用指南

date: 2017-11-10 19:42:37

tags: 其它
---

环境： Mac
Charles版本：v4.1.3

charles是Mac上很好用的抓包工具。在需要跨页面的查看所有请求时，Chrome的network已经不能满足这个要求，此时使用charles再好不过了。另外还可以使用charles更改请求和返回数据、使请求打到不同的环境等。下面介绍几个常用的技巧。

<!--more-->

#### Recording settings
打开Proxy->Recording Settings，有以下三个选项

1.options

![](http://note.youdao.com/yws/public/resource/a9acf0b7cd75f096932b413d3307b10f/xmlnote/WEBRESOURCE91a698175012928073c4f5ae3685fc67/973)

Max Requests: 限制当前存储的最多请求数，超过这个数字的日期较早的请求将会被自动清除掉。再也不用手动点清除按钮了

2.Include

![](http://note.youdao.com/yws/public/resource/a9acf0b7cd75f096932b413d3307b10f/xmlnote/WEBRESOURCE0af1bfea9e344a793c93034eaa99d859/977)

太多的请求眼花缭乱，看不到自己想看到的请求？那么使用Include吧。只要你在这里配置了请求，那么charles将只拦截匹配的请求，还可以使用正则匹配，例如图片中，只拦截域名以"baidu.com"结束的域名，www.baidu.com、news.baidu.com以及zhidao.baidu.com都可以拦截到，但是不想看到的例如www.taobao.com就看不到了

3.Exclude

![](http://note.youdao.com/yws/public/resource/a9acf0b7cd75f096932b413d3307b10f/xmlnote/WEBRESOURCEaf03c2b6daaacb1bf0c63901ee4cd20e/981)

也许你只想拦截www.baidu.com、news.baidu.com以及别的*.baicu.com而不想看到zhidao.baidu.com呢？那就include \*.baidu.com，配置exclude zhidao.baidu.com吧


#### Map Remote

开发过程中经常遇到需要把远端请求打到本地的情况。尤其是有了node之后，这种需求愈发明显

例如有以下情况

原始请求:http://news.baidu.com/tech/category1/widget?ajax=json&id=ad
本地路径:localhost:8080/pages/widget?ajax=json&id=ad

如果通用的匹配路径可归纳为将http://news.baidu.com/tech/category1/(abc) 匹配至 localhost:8080/pages/abc

那么可以如下配置：

![](http://note.youdao.com/yws/public/resource/a9acf0b7cd75f096932b413d3307b10f/xmlnote/WEBRESOURCE72abf6622522bd0d92d141d1d97a0bc9/983)

#### BreakPoints

开发过程中还经常要模拟不同情况的返回值，在百度新闻刷新页面时，会调这么一个接口

Get: http://news.baidu.com/passport

下面介绍怎么修改它的请求值和返回值，因为这个接口无关紧要，只是在真正登录时返回了用户的用户名供页面展示，一般不会有安全的问题，所以可以直接拦截到，此处只是拿它举一个例子：如何修改请求及返回值。

用charles拦截到这个请求后，右键-->BreakPoints
![](http://note.youdao.com/yws/public/resource/a9acf0b7cd75f096932b413d3307b10f/xmlnote/WEBRESOURCE5936e72de2791e2a37dc25ef59b07286/985)

> 注：此处右键之后有很多的功能待发掘，例如repeat会重复发送这个请求，或者此处有Map Remote可以直接进入界面设置要把这个路径匹配至哪里等等

下次再刷新页面时，会出现如下界面
![](http://note.youdao.com/yws/public/resource/a9acf0b7cd75f096932b413d3307b10f/xmlnote/WEBRESOURCE8c4e41076b9fb5f3b2048d14e238da7d/987)

有一个Edit Request，在这个界面可以增加或删除参数，或者直接双击name或者value进行编辑，修改完参数之后，点击Execute。有返回之后，会出现如下界面

![](http://note.youdao.com/yws/public/resource/a9acf0b7cd75f096932b413d3307b10f/xmlnote/WEBRESOURCEb949ddcbddd4708ce787568915c4a510/989)

界面中有edit Response，同样可以直接修改，修改完之后，点击Execute，请求就完成了，返回的结果可以直接供前端来使用。

![](http://note.youdao.com/yws/public/resource/a9acf0b7cd75f096932b413d3307b10f/xmlnote/WEBRESOURCE7d1896954185fe07ac60e5a449c057b6/991)