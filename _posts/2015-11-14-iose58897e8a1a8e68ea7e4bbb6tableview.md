---
ID: 109
post_title: iOS列表控件TableView
author: HugeTerry
post_excerpt: ""
layout: post
permalink: http://hugeterry.cn/dreams/109
published: true
post_date: 2015-11-14 22:12:20
---
今天学习了TableView的一些使用方法，总体来说和Android的listview大致的结构是差不多的，主要是内部使用的方法有一些比较大的区别
<h1>TableView的使用</h1>
首先在storyboard中拖拽铺满TableView，创建一个继承UITableView的类后，重写init方法，在storyboard绑定类后重写以下三个方法（必须）
<ul>
	<li>numberOfSectionsInTableView: 告知视图，有多少个section需要加载到table里</li>
	<li>tableView:numberOfRowsInSecion:  告知controller每个section需要加载多少个单元或多少行</li>
	<li>tableView:cellForRowAtIndexPath: 返回UITableViewCell的实例，用于构成table view.</li>
</ul>
接着在storyboard中拖进Table View Cell，在右边菜单Identifiel给其一个id，这次案例给其id为"cell"

在Table View Cell上方拖进label，用于显示文字，后在label右边菜单View栏中Tag改为1

实现<span class="s1">UITableViewDelegate接口，在init方法中</span><code><span class="s1">self</span><span class="s2">.</span><span class="s3">dataSource</span><span class="s2"> = </span><span class="s1">self</span></code>

<span class="s1">给TableView提供相应的数据</span>

之后修改上面重写三个方法传入参数即可

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-19.49.23.png"><img class="alignnone size-medium wp-image-111" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-19.49.23-300x216.png" alt="屏幕快照 2015-11-14 19.49.23" width="300" height="216" /></a><a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-21.35.57.png"><img class="alignnone wp-image-113" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-21.35.57-162x300.png" alt="屏幕快照 2015-11-14 21.35.57" width="117" height="217" /></a>

这时一个简单的TableView就显示了出来，接下来再对其进一步修改
<h1>创建多个Section列表</h1>
<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-21.46.17.png"><img class=" wp-image-114 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-21.46.17-167x300.png" alt="屏幕快照 2015-11-14 21.46.17" width="106" height="190" /></a>

从iOS设置中看到的setting列表中每一块即为一个section

其中刚才写的numberOfSectionsInTableView方法返回的参数就是为了告知视图TableView中有几个section
<ul>
	<li class="p1"><span class="s1">tableView:titleForHeaderInSection ：给每一个section指明一个标题</span></li>
</ul>
使用这个方法即可设置标题

新建一个Resource文件，选择Property List，写入数据。<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-22.01.42.png"><img class="alignnone size-medium wp-image-115" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-22.01.42-300x70.png" alt="屏幕快照 2015-11-14 22.01.42" width="300" height="70" /></a><span style="text-decoration: underline;">注意类型</span>

定义一个NSDictionary变量后获取上述文件内容，之后改变各个方法传入返回的参数，达到右图效果

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-20.33.42副本.png"><img class="alignnone size-medium wp-image-110" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-20.33.42副本-300x285.png" alt="屏幕快照 2015-11-14 20.33.42副本" width="300" height="285" /></a><a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-22.05.43.png"><img class="alignnone wp-image-116" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-22.05.43-162x300.png" alt="屏幕快照 2015-11-14 22.05.43" width="154" height="285" /></a>
<h1>实现列表交互</h1>
首先实现<span class="s1">UITableViewDelegate接口</span>

接着在init方法中传入<code><span class="s1">self</span><span class="s2">.</span><span class="s3">delegate</span><span class="s2"> = </span><span class="s1">self</span></code>
<ul>
	<li>tableview:<span class="s1">didSelectRowAtIndexPath ：TableView中sell的触发事件方法</span></li>
</ul>
<blockquote>这个方法有点类似Android中listview的点击监听器，同样将要实现的功能写入方法中则可以达到想要的效果</blockquote>
在上述代码中加入此方法后则完成点击触发事件

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-22.58.17.png"><img class="alignnone size-medium wp-image-119" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-22.58.17-300x24.png" alt="屏幕快照 2015-11-14 22.58.17" width="300" height="24" /></a>

每次点击tableview后控制台出现的效果：

<a href="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-23.01.09.png"><img class="alignnone size-medium wp-image-120" src="http://www.hugeterry.cn/wp-content/uploads/2015/11/屏幕快照-2015-11-14-23.01.09-300x87.png" alt="屏幕快照 2015-11-14 23.01.09" width="300" height="87" /></a>

&nbsp;

&nbsp;