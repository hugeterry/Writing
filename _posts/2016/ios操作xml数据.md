---
ID: 297
post_title: iOS操作XML数据
post_name: 'ios%e6%93%8d%e4%bd%9cxml%e6%95%b0%e6%8d%ae'
author: HugeTerry
post_date: 2016-01-20 17:21:53
layout: post
link: http://hugeterry.cn/dreams/297
published: true
tags:
  - iOS
  - Swift
categories:
  - iOS
---
<blockquote>在Android里边，解析xml是必不可少的，同样的iOS也是一个道理

联网获取数据或多或少有xml格式的解析</blockquote>
xml数据

<a href="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2016-01-19-16.21.40.png" rel="attachment wp-att-298"><img class="alignnone size-full wp-image-298" src="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2016-01-19-16.21.40.png" alt="屏幕快照 2016-01-19 16.21.40" width="657" height="167" /></a>

&nbsp;
<h1>通过swift解析XML格式的数据</h1>
照惯例，贴代码

<a href="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2013-01-01-08.11.54.png" rel="attachment wp-att-301"><img class="alignnone size-full wp-image-301" src="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2013-01-01-08.11.54.png" alt="屏幕快照 2013-01-01 08.11.54" width="759" height="216" /></a>

这个时候需要解析各节点之间的名字

<a href="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2016-01-20-17.10.32.png" rel="attachment wp-att-302"><img class="alignnone size-full wp-image-302" src="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2016-01-20-17.10.32.png" alt="屏幕快照 2016-01-20 17.10.32" width="737" height="271" /></a>

去除多余的空格，提取有用的值

<a href="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2016-01-20-17.11.46.png" rel="attachment wp-att-304"><img class="alignnone size-full wp-image-304" src="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2016-01-20-17.11.46.png" alt="屏幕快照 2016-01-20 17.11.46" width="772" height="197" /></a>

&nbsp;

那么，这个时候控制台所解析的数据为<a href="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2016-01-20-17.13.49.png" rel="attachment wp-att-305"><img class="alignnone size-full wp-image-305" src="http://www.hugeterry.cn/wp-content/uploads/2016/01/屏幕快照-2016-01-20-17.13.49.png" alt="屏幕快照 2016-01-20 17.13.49" width="423" height="114" /></a>

&nbsp;

&nbsp;