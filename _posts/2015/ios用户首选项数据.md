---
ID: 217
post_title: iOS用户首选项数据
post_name: 'ios%e7%94%a8%e6%88%b7%e9%a6%96%e9%80%89%e9%a1%b9%e6%95%b0%e6%8d%ae'
author: HugeTerry
post_date: 2015-12-15 20:01:59
layout: post
link: http://hugeterry.cn/dreams/217
published: true
tags:
  - iOS
  - Swift
categories:
  - iOS
---
<blockquote>本地存储数据简单的说有三种方式：数据库、NSUserDefaults和文件

NSUserDefaults用于存储数据量小的数据，例如用户配置

类似于Android中的sharedpreferences</blockquote>
<h1>读写用户首选项数据</h1>
首先在storyboard中加入两个控件textview与button，用来显示数据及保存数据

接下来，按照国际惯例，贴代码

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2013-01-01-09.13.30.png"><img class="size-full wp-image-218 alignnone" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2013-01-01-09.13.30.png" alt="屏幕快照 2013-01-01 09.13.30" width="760" height="460" /></a>

&nbsp;
<h1>启动时小贴士实例</h1>
首先在storyboard添加switch用来标识是否启动小贴士<img class="wp-image-219 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2013-01-01-10.25.12.png" alt="屏幕快照 2013-01-01 10.25.12" width="230" height="141" />

event设置为value changed，值改变时触发事件

接着，贴代码

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2013-01-01-11.10.59.png"><img class="alignnone size-full wp-image-220" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2013-01-01-11.10.59.png" alt="屏幕快照 2013-01-01 11.10.59" width="763" height="447" /></a>

添加swich事件后，添加展示提示框代码

&nbsp;