---
ID: 179
post_title: iOS 屏幕适配
post_name: 'ios-%e5%b1%8f%e5%b9%95%e9%80%82%e9%85%8d'
author: HugeTerry
post_date: 2015-12-02 21:10:30
layout: post
link: http://hugeterry.cn/dreams/179
published: true
tags:
  - iOS
  - Swift
categories:
  - iOS
---
iOS的屏幕适配相比于Android的多种分辨率屏幕简单得多，在设置和配置方面也是易操作的。

本次测试均采用Xcode7，而苹果官方在匹配方面也改变了不少功能，例如本例使用的Editor-&gt;Pin菜单在Xcode6及以下版本的功能均废除了。同时Xcode7也引入了Stack View，这也使得在开发中多了一些新特性
<h1>匹配父级容器</h1>
<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-02-15.39.58.png"><img class=" wp-image-181 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-02-15.39.58.png" alt="屏幕快照 2015-12-02 15.39.58" width="268" height="278" /></a>

匹配父级容器的好处在于可以让控件匹配所有的屏幕，包括iPad，iPhone等
<blockquote>使用的是Xcode7，相比于Xcode6没有了Editor-&gt;Pin菜单

那么在Xcode6下可使用Editor-&gt;Pin菜单对边距进行调整达到匹配效果</blockquote>
<strong>对于Xcode7，有如右图方法</strong>

<strong>选择View将四个方向的相对于SuperView的边距调整设置约束即可</strong>

&nbsp;
<h1>分割父级容器</h1>
<img class=" wp-image-182 alignright" style="line-height: 1.5;" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-02-17.48.57.png" alt="屏幕快照 2015-12-02 17.48.57" width="281" height="297" />

分割父级容器的目的在于让控件之间比例在不同设备中都能匹配
<h2>Xcode6旧方法匹配界面</h2>
在Xcode6下，可以选中Editor-&gt;Pin-&gt;Horizontal Spacing对两个控件进行绑定，然后根据匹配父级容器的方法设置左右边距使其匹配各界面

点击Editor-&gt;Pin-&gt;Widths Equally后在右边栏的Multiplier可调整比例，点击Constant可设置边距数值。

在First Item和Second Item分别是两个控件的设置声明
<h2>Xcode7 Stack View匹配界面</h2>
Stack View类似于Android中的LinearLayout，都是线性的属性，多多少少有些相似
<h3>添加子视图View并将其合并Stack View</h3>
<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-02-19.15.53.png"><img class=" wp-image-186 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-02-19.15.53.png" alt="屏幕快照 2015-12-02 19.15.53" width="293" height="217" /></a>点击右下角第一个图标<a href="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-02-19.17.27.png"><img class="alignnone size-full wp-image-187" src="http://www.hugeterry.cn/wp-content/uploads/2015/12/屏幕快照-2015-12-02-19.17.27.png" alt="屏幕快照 2015-12-02 19.17.27" width="25" height="24" /></a>，将其两个View合并为一个stackView 组，此时会看到两个View呈现紧缩状态，

<strong>修改StackView 属性</strong>

在右边属性面板中，修改水平间距Spacing值，可看到两个View间出现间隙，并将Distribution 调整为 Fill Equally 值（等分填充）
<h3>添加约束</h3>
接下来，根据匹配父级容器的方式点击右下角第三个按钮添加约束

&nbsp;

这是简单的屏幕匹配，学习之后多几层嵌套熟悉一下StackView及约束属性