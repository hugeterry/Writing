---
ID: 159
post_title: iOS常用项目模板
post_name: 'ios%e5%b8%b8%e7%94%a8%e9%a1%b9%e7%9b%ae%e6%a8%a1%e6%9d%bf'
author: HugeTerry
post_date: 2015-11-21 16:01:22
layout: post
link: http://hugeterry.cn/dreams/159
published: true
tags:
  - iOS
  - Swift
categories:
  - iOS
---
Xcode提供的项目模板有五个，与Android开发比较起来，这种方式让开发节省了不少开发时间，以下一一列举说明

Master-Detail Application：日记类App

Page-Based Application：电子书类App

Tabbed Application：导航栏App
<h1>Master-Detail Application</h1>
<p class="p1"><span class="s1">从viewDidLoad()能看到按钮的声明，注释即可隐藏<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-13.57.48.png"><img class="alignnone size-full wp-image-161" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-13.57.48.png" alt="屏幕快照 2015-11-21 13.57.48" width="801" height="90" /></a></span></p>
<p class="p1">通过<code>tableview：</code><span class="s1"><code>canEditRowAtIndexPath</code> 返回的值设定可让tableview是否呈现拖动后按钮</span></p>
<p class="p1"><a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-14.01.10.png"><img class="alignnone size-full wp-image-162" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-14.01.10.png" alt="屏幕快照 2015-11-21 14.01.10" width="736" height="86" /></a></p>
<p class="p1">接下来是呈现删除功能的方法<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-14.04.01.png"><img class="alignnone size-full wp-image-163" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-14.04.01.png" alt="屏幕快照 2015-11-21 14.04.01" width="729" height="157" /></a></p>
可通过注释和修改以上主要方法，实现想要的功能

&nbsp;
<h1>Page-Based Application</h1>
Page-Based Application的修改因注意Xcode的版本，我使用的版本为7.1正式版，在使用的过程中查阅过一些资料，6.0版本有一些参数是不需要更改的
<h2>更改分页标题</h2>
此框架中ModelController.swift为数据源的设置文件<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-14.30.39.png"><img class="alignnone size-full wp-image-164" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-14.30.39.png" alt="屏幕快照 2015-11-21 14.30.39" width="613" height="115" /></a>

将原有的提取月份注释掉，传入一个参数数组进去，则可将每个翻页标题更改
<h2>内容及标题更改</h2>
<img class="size-full wp-image-165 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-14.27.34.png" alt="屏幕快照 2015-11-21 14.27.34" width="250" height="145" />
<h3>设置绑定<span class="s1">TextView</span></h3>
为storyboard中Data View中添加textview后，将Behavior中Edutable属性设置为false，为了让文本不可编辑

之后将textview拖拽绑定到DataViewController.swift中绑定
<h3>配置pageData</h3>
<h4>1.pageData传入字典</h4>
在ModelController.swift文件中配置复杂的pageData数组中，尝试将字典加入数组中

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.10.18.png"><img class="alignnone size-full wp-image-167" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.10.18.png" alt="屏幕快照 2015-11-21 15.10.18" width="601" height="197" /></a>
<h4>2.修改pageData类型</h4>
跟前一个方法不同的是，通过加入字典向textview传入内容，此时应将pageData的类型String转化为NSArray
<h3>修改传入<span style="line-height: 1.5;">data view controller</span>值</h3>
<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.18.17.png"><img class="alignnone size-full wp-image-168" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.18.17.png" alt="屏幕快照 2015-11-21 15.18.17" width="745" height="71" /></a>

由于pageData改为含字典数组，需改传回data view controller的值，及使用<code>indexofObject()</code>
<h3>将pageData展示到界面中</h3>
<h4>1.声明控件及变量类型设置</h4>
首先注意拖拽的textview是否已经绑定

注意传入的数据源pageData是数组，需修改<span class="s1">dataObject的String类型改为</span><span class="s1">AnyObject<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.22.20.png"><img class="alignnone size-full wp-image-169" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.22.20.png" alt="屏幕快照 2015-11-21 15.22.20" width="476" height="72" /></a></span>
<h4>2.修改控件展示的值</h4>
<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.26.44.png"><img class="alignnone size-full wp-image-170" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.26.44.png" alt="屏幕快照 2015-11-21 15.26.44" width="664" height="106" /></a>

&nbsp;
<h1>Tabbed Application</h1>
Tabbed Application其实类似于Android开发中的导航栏，相比于Android的Fragment+ViewPager开发起来会舒服很多，原因在于只需要套用这个框架后轻轻配置即可

<img class=" wp-image-171 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.47.32.png" alt="屏幕快照 2015-11-21 15.47.32" width="139" height="137" />

新建文件选择Tabbed Application框架后在storyboard中拖拽一个新的ViewControllers

从Tab Bar Contrpller中心页面拖拽至新建ViewControllers，在弹出窗口选择view controllers

接着在右边栏点击新建的ViewControllers的标签选项<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.50.12.png"><img class="wp-image-172 alignleft" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.50.12.png" alt="屏幕快照 2015-11-21 15.50.12" width="209" height="127" /></a><img class="size-full wp-image-174 alignleft" style="line-height: 1.5;" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-21-15.51.32.png" alt="屏幕快照 2015-11-21 15.51.32" width="261" height="127" />

&nbsp;

&nbsp;

&nbsp;

&nbsp;

接下来，好好享用导航栏吧