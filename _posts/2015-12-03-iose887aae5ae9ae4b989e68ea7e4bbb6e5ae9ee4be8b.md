---
ID: 189
post_title: iOS自定义控件实例
author: HugeTerry
post_excerpt: ""
layout: post
permalink: http://hugeterry.cn/dreams/189
published: true
post_date: 2015-12-03 20:38:48
---
在进行了屏幕适配后，接下来是一些控件的特性和带来的便利
<h1>配置iOS自定义控件属性</h1>
<img class=" wp-image-190 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-18.49.43.png" alt="屏幕快照 2015-12-03 18.49.43" width="222" height="325" />

iOS相比Android的控件属性设置起来方便得多，Android要使用xml进行配置，相比之下，iOS只需操作即可设置属性

以Button为例：

在右边属性面板中，State Config可设置Button的当前状态
<ul>
	<li>Default：常规状态显现</li>
	<li>Highlighted：高亮状态显现</li>
	<li>Selected：选中状态</li>
	<li>Disabled：禁用状态</li>
</ul>
一般来说使用Default与Highlighted搭配常规与被点击呈现

在Background可设置按钮背景进行比较，其他属性均可对其进行设置

&nbsp;
<h1>自定义圆形进度指示控件</h1>
<blockquote>使用绘图API来自定义控件</blockquote>
<h2>创建一个绘图类ProgressController</h2>
<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-19.47.57.png"><img class="alignnone size-full wp-image-191" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-19.47.57.png" alt="屏幕快照 2015-12-03 19.47.57" width="641" height="360" /></a><a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-19.48.08.png"><img class="alignnone size-full wp-image-192" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-19.48.08.png" alt="屏幕快照 2015-12-03 19.48.08" width="761" height="304" /></a>

注意drawRect方法中对绘图API的使用
<h2>声明显示圆形进度指示控件</h2>
接下来，在StoryBoard中拖拽一个button创建action事件

在ViewController设置声明显示它既可

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-19.47.18.png"><img class="alignnone size-full wp-image-193" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-19.47.18.png" alt="屏幕快照 2015-12-03 19.47.18" width="512" height="403" /></a>

&nbsp;
<h1>StoryBoard中实时预览自定义控件效果</h1>
这个功能在Xcode6中出现，更多的是让程序猿与射鸡师实现无缝连接，程序猿可以通过写好的storyboard右边属性让射鸡师进行页面设计
<h2>新建与绑定界面</h2>
新建项目后在面板左上角新建一个cocoa touch framework，接着新建一个class文件

在main.storyboard与新建的class文件通过custom class属性进行绑定
<h2>编写属性对应代码</h2>
接着开始对class文件做文章

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-20.33.21.png"><img class="alignnone size-full wp-image-194" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-20.33.21.png" alt="屏幕快照 2015-12-03 20.33.21" width="580" height="602" /></a>

新建属性后在storyboard可以看到<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-20.32.36.png"><img class="alignnone size-full wp-image-195" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-03-20.32.36.png" alt="屏幕快照 2015-12-03 20.32.36" width="572" height="366" /></a>

多出了str，边框宽度，边框颜色，圆角属性。诸如此类，其他属性也可以通过这种方法设置。