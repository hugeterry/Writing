---
ID: 613
post_title: Android中的位运算
author: HugeTerry
post_excerpt: ""
layout: post
permalink: http://hugeterry.cn/dreams/613
published: true
post_date: 2017-09-01 20:40:36
---
最近在开发中见到了位运算，揣摩了许久，总结了一下

学会从开发中运用位运算，首先要懂得他的思想,这个就涉及一道常见的题目:<a href="https://www.zhihu.com/question/19676641" target="_blank" rel="noopener">有 1000 个一模一样的瓶子，其中有 999 瓶是普通的水，有一瓶是毒药。任何喝下毒药的生物都会在一星期之后死亡。现在，你只有 10 只小白鼠和一星期的时间，如何检验出哪个瓶子里有毒药？</a>
<div>
<blockquote>
<div>答案是这样的：</div>
<div>1,把1000瓶标号：1,2,3,4,5,6...1000.
2,所有老鼠排列在一起组成一个2进制队列: 0000000000
0代表不喝，1代表喝
3,0000000001代表第一瓶水被喝情况
0000000010代表第二瓶水被喝情况
0000000011代表第三瓶水被喝情况
0000000100代表第四瓶水被喝情况
...
1111101000代表第1000瓶水被喝情况
4,第7天，喝了毒药的老鼠都死了，那个二进制队列转为为十进制就是毒药的标号。
比如第3只老鼠死亡，其他老鼠没死，队列为0000000100，第四瓶水有毒。
第1，5，6，8老鼠死亡，其他没死，队列为0010110001，第177瓶水有毒。</div>
<div></div>
作者：kirch链接：https://www.zhihu.com/question/19676641/answer/14123096来源：知乎著作权归作者所有</blockquote>
</div>
通过这个问题第一二个问答懂得解题的方式后,也就懂得使用位运算的思想

在Android源码里边,有很多运用到位运算的地方,如Intent Flags:
<pre><code>public static final int FLAG_ACTIVITY_NEW_TASK = 0x10000000;
public static final int FLAG_ACTIVITY_SINGLE_TOP = 0x20000000;
public static final int FLAG_ACTIVITY_MULTIPLE_TASK = 0x08000000;</code></pre>
所以我们在添加intent时会使用这种写法：
<pre><code>mIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK| Intent.FLAG_ACTIVITY_RESET_TASK_IF_NEEDED| Intent.FLAG_ACTIVITY_SINGLE_TOP);</code></pre>
<blockquote>这里可能涉及到一个问题:为什么使用十六进制

其实在android的R文件中你也会看到都是使用十六进制作为数据，主要原因是16进制比较方便转换成二进制，因为c语言非常多的数据运行需要使用位运算，位运算就必然就是要转化成二进制。那为什么不直接使用二进制呢？原因是二进制书写太容易出错，又长。按十六进制和二进制来说、可以一一按位转换、比如:

0x4EF  -&gt;  0100 1110 1111

0x4C5596 -&gt;  0100 1100 0101 0101 1001 0110

参考：<a href="http://www.cnblogs.com/200911/p/3348371.html?utm_source=tuicool" target="_blank" rel="noopener">为什么android的R类要定义成16进制</a></blockquote>
而当我们自己执行一些api操作时,像在android 6.0+ 改变状态栏:

白底黑字:
<pre><code>if (Build.VERSION.SDK_INT &gt;= Build.VERSION_CODES.M) {
activity.getWindow().getDecorView().setSystemUiVisibility( View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN|View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR);
}
</code></pre>
黑底白字:
<pre><code>if (Build.VERSION.SDK_INT &gt;= Build.VERSION_CODES.M) {
activity.getWindow().getDecorView().setSystemUiVisibility( View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN &amp; ~FLAG_SYSTEM_UI_FLAG_LIGHT_STATUS_BAR; 
}</code></pre>
&nbsp;

结合上面的思想，就有了以下运用：
<h3><strong>添加属性（或运算）：</strong></h3>
android6.0+ 状态栏白底黑字：
<pre><code>if (Build.VERSION.SDK_INT &gt;= Build.VERSION_CODES.M) {
activity.getWindow().getDecorView().setSystemUiVisibility( View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN|View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR);
}</code></pre>
添加长按监听状态：
<pre><code>event.mFlags |= FLAG_CANCELED | FLAG_CANCELED_LONG_PRESS;</code></pre>
android源码例子：
<pre><code>baseIntent = Intent.parseUri(arg, Intent.URI_INTENT_SCHEME| Intent.URI_ANDROID_APP_SCHEME | Intent.URI_ALLOW_UNSAFE);</code></pre>
&nbsp;
<h3><strong>删除属性（&amp;～）：</strong></h3>
android6.0+ 状态栏更改为黑字：
<pre><code>if (Build.VERSION.SDK_INT &gt;= Build.VERSION_CODES.M) {
activity.getWindow().getDecorView().setSystemUiVisibility( View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN &amp; ~FLAG_SYSTEM_UI_FLAG_LIGHT_STATUS_BAR; 
}</code></pre>
android源码例子：
<pre><code>intent.mFlags &amp;= ~IMMUTABLE_FLAGS;</code></pre>
&nbsp;
<h3><strong>判断是否包含该属性（与运算结果为0：不包含；结果为子属性：包含）：</strong></h3>
判断视图是否为disable 这里ENABLED_MASK的值与 DISABLED的值相同：
<pre><code>if ((viewFlags &amp; ENABLED_MASK) == DISABLED) {  
    ...  
｝ </code></pre>
android源码例子：
<pre><code>if ((flags&amp;URI_INTENT_SCHEME) != 0) {
            uri.append("intent:");
}</code></pre>
&nbsp;

以上是关于android中源码以及实际应用的位运算常见的运用