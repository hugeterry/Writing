---
ID: 136
post_title: iOS UI界面跳转
post_name: 'ios-ui%e7%95%8c%e9%9d%a2%e8%b7%b3%e8%bd%ac'
author: HugeTerry
post_date: 2015-11-18 01:42:18
layout: post
link: http://hugeterry.cn/dreams/136
published: true
tags:
  - iOS
  - Swift
categories:
  - iOS
---
<blockquote>关于界面跳转在<a href="http://hugeterry.cn/dreams/87">iOS界面开发基本流程</a>一文中视图切换已经初步的说明过，接下来是比较深入的探讨</blockquote>
<h1>借助StoryBoard做跳转</h1>
<h2>跳转动画效果</h2>
跳转动画效果在ctrl拖拽实现modal切换后，右边菜单Transition可选择动画效果进行添加。

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-17-12.09.54.png"><img class=" wp-image-137 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-17-12.09.54.png" alt="屏幕快照 2015-11-17 12.09.54" width="241" height="165" /></a>
<ul>
	<li>Cover Vertical ：垂直覆盖</li>
	<li>Filp Horizontal：水平翻转</li>
	<li>Cross Dissoive：淡入淡出</li>
	<li>Partial Curl：翻纸效果</li>
</ul>
<h2>返回</h2>
返回效果类比Android还是有几分相似

其中<code><span class="s1">dismissModalViewControllerAnimated </span></code><span class="s1">是苹果在iOS6.0后抛弃的</span>

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-17-22.42.09.png"><img class="alignnone size-full wp-image-138" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-17-22.42.09.png" alt="屏幕快照 2015-11-17 22.42.09" width="591" height="68" /></a>
<p class="p1"><code><span class="s1">dismissViewControllerAnimated </span></code><span class="s1">其中第一个boolean参数是指是否使用动画，第二个传进nil</span></p>
&nbsp;
<h1>使用nib文件做iOS界面设计</h1>
xcode4之前界面是用Interface Builder来做界面的，那么新建一个文件<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-17-23.18.36.png"><img class="size-medium wp-image-139 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-17-23.18.36-300x213.png" alt="屏幕快照 2015-11-17 23.18.36" width="300" height="213" /></a>

此时应勾选Also Create XIB file

那么新建的文件会看到有对应的swift文件以及xib文件
<h2>绑定控件</h2>
xib是用来展示界面的，那么与通常方法相同的是，可以用拖拽让组件或者事件直接建在对应的xib文件上
<h2>视图切换</h2>
视图切换与平时会有较大的差异，切换时类似于Android中的intent

使用的方法为<code><span class="s1">presentViewController </span></code><span class="s1">传入的参数为</span>
<ul>
	<li class="p1"><span class="s1">MyViewController</span><span class="s2">(nibName:跳转到nib文件的名字，不加后缀名</span><span class="s2">,bundle: </span><span class="s4">nil</span><span class="s2">)</span></li>
	<li class="p1"><span class="s2"> animated: 是否呈现动花效果</span></li>
	<li class="p1"><span class="s2">completion: </span><span class="s4">nil</span></li>
</ul>
<img class="alignnone size-full wp-image-141" style="line-height: 1.5" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-17-23.24.55.png" alt="屏幕快照 2015-11-17 23.24.55" width="761" height="85" />同样的在nib文件中拖拽即可添加事件，写入<code>dismissViewControllerAnimated</code>可实现返回

&nbsp;
<h1>传递数据</h1>
传递数据在Android中运用到的是intent的putExtra()方法，那么在iOS开发中，传递数据只需拿到跳转页面的ViewController后拿到公开的变量进行赋值。跳转后拿出此变量使用即可。

<a style="line-height: 1.5" href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-18-01.39.33.png"><img class="alignnone size-medium wp-image-146" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-18-01.39.33-300x177.png" alt="屏幕快照 2015-11-18 01.39.33" width="300" height="177" /></a><a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-18-01.40.59.png"><img class="alignnone size-medium wp-image-145" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-18-01.40.59-300x177.png" alt="屏幕快照 2015-11-18 01.40.59" width="300" height="177" /></a>

以上分别是首页和次页传递数据使用的方法

&nbsp;