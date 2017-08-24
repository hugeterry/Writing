---
ID: 292
post_title: >
  关于Android
  notifyDataSetChanged()遇到的一坑
author: HugeTerry
post_excerpt: ""
layout: post
permalink: http://hugeterry.cn/dreams/292
published: true
post_date: 2016-01-17 17:04:37
---
今天在一个留言APP重写的过程中遇到了一个大坑，留言版的APP如<a href="http://www.hugeterry.cn/wp-content/uploads/2016/01/使用.jpg" rel="attachment wp-att-293"><img class="wp-image-293 alignright" src="http://www.hugeterry.cn/wp-content/uploads/2016/01/使用.jpg" alt="使用" width="192" height="292" /></a>右图，首先登陆跳转到这个界面后是连接服务器获取数据，调用一次notifyDataSetChanged()更新listview。

那么当在输入框填写东东按发送后，需再进行一次更新。

之前用的方法是通过recreate()让整个activity重新载入。

重写的时候用到第二次的notifyDataSetChanged()测试了一天一直没法实现，本来以为问题出在handler，loop部分。下午看了一堆资料后才发现这个大坑。

&nbsp;

要使用如下代码进行第二次的notifyDataSetChanged()更新

<code>lists = new ArrayList&lt;Map&lt;String, Object&gt;&gt;();//第一次刷新listview时adapter加载的是lists</code>

<code>list = new ArrayList&lt;Map&lt;String, Object&gt;&gt;();</code>

<code>map.put...</code>

<code>...</code>

<code>map.put...</code>

<code>...</code>

<code>list.add(map);</code>

<code>lists.clear();</code>

<code>lists.addAll(list);</code>

<code>ad.notifyDataSetChanged();</code>

&nbsp;

原因在于：对于ArrayList来说，执行一次add方法，并不会将add的元素指向一个堆里边，而是指向不同的堆中，所以当第二次进行更新时，就碰到了坑。此时应该新建一个ArrayList,将需add的元素加入里边，然后清除原来第一个Arraylist的数据，将第二个Arraylist的所有元素加入到第一个中，再进行刷新

&nbsp;