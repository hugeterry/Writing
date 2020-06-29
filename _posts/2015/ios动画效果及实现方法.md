---
ID: 202
post_title: iOS动画效果及实现方法
post_name: 'ios%e5%8a%a8%e7%94%bb%e6%95%88%e6%9e%9c%e5%8f%8a%e5%ae%9e%e7%8e%b0%e6%96%b9%e6%b3%95'
author: HugeTerry
post_date: 2015-12-08 20:56:02
layout: post
link: http://hugeterry.cn/dreams/202
published: true
tags:
  - iOS
  - Swift
categories:
  - iOS
---
该例中动画效果的实现前几步大致相同，主要考察<code>touchesBegan</code>中各种方法的使用以实现自己想要的动画效果
<h1>视图切换</h1>
<img class="size-full wp-image-203 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-08-19.58.42.png" alt="屏幕快照 2015-12-08 19.58.42" width="270" height="158" />
<h2>在storyboard与类中声明切换的控件</h2>
首先在StoryBoard中创建两个imagview拖拽至控件面板，用来做切换效果

&nbsp;
<h2>视图切换相关代码</h2>
首先，声明代码和显示相关控件

定义一个布尔值来判断是否正面，实现点击返回效果<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-08-20.02.33.png"><img class="alignnone size-full wp-image-205" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-08-20.02.33.png" alt="屏幕快照 2015-12-08 20.02.33" width="754" height="210" /></a>

接下来，是点击方法的运用<code><span class="s1">transitionFromView</span></code>
<ul>
	<li>与Android的intent相似，第一个和第二个参数分别为切换前和切换后的控件</li>
	<li>第三个参数duration为切换时间</li>
	<li>第四个参数options为切换方式</li>
	<li>第五个参数用来插入切换视图完成后执行的方法，该例子执行<code>completeHandler</code>方法<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-08-20.04.16.png"><img class="size-full wp-image-206 alignnone" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-08-20.04.16.png" alt="屏幕快照 2015-12-08 20.04.16" width="822" height="318" /></a></li>
</ul>
此时，视图切换完成

&nbsp;
<h1>视图动画效果</h1>
国际惯例，视图动画效果同样的与切换视图的前奏一样，storyboard与类中声明切换的控件，操作是完全相同的，接下来，贴出实现代码<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-08-20.26.06.png"><img class="alignnone size-full wp-image-207" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-08-20.26.06.png" alt="屏幕快照 2015-12-08 20.26.06" width="826" height="390" /></a>

其中，关键在于<code>touchesBegan</code>方法
<p class="p1"><span class="s1">beginAnimations：开始配置动画</span></p>
<p class="p1"><span class="s1">setAnimationTransition：配置动画</span></p>

<ul>
	<li class="p1">第一个参数：动画效果，图中动画效果为翻页</li>
	<li class="p1">第二个参数：动画控件</li>
	<li class="p1">第三个参数：是否缓存</li>
</ul>
<p class="p1"><span class="s1">setAnimationDuration：设置动画时间</span></p>
<p class="p1"><span class="s1">commitAnimations：提交动画</span></p>
&nbsp;
<h1 class="p1">自定义动画</h1>
国际惯例，storyboard与类中声明切换的控件，操作与前面相同

国际惯例，接下来还是贴代码<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-08-20.46.21.png"><img class="alignnone size-full wp-image-208" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-08-20.46.21.png" alt="屏幕快照 2015-12-08 20.46.21" width="778" height="453" /></a>

还是同样的配方，使用<code>touchesBegan</code>方法来实现点击的效果

接下来使用的是<code><span class="s1">transitionWithView，</span></code><span class="s1">与切换视图不同，该方法是一个控件的点击效果，参数大致相同，第一个参数为控件，第二个为切换时间，第三个切换效果，第四个切换方法，第五个切换后执行方法</span>

那么在anim方法中，
<ul>
	<li>alpha：点击后透明度变化</li>
	<li><span class="s1">CGPoint： 点击后坐标变化，通过中心坐标变化来使整张图移动</span></li>
</ul>
此时即实现了自定义动画