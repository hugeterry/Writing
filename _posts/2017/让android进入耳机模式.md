---
ID: 597
post_title: 让Android手机强制进入耳机模式
post_name: '%e8%ae%a9android%e8%bf%9b%e5%85%a5%e8%80%b3%e6%9c%ba%e6%a8%a1%e5%bc%8f'
author: HugeTerry
post_date: 2017-08-16 21:43:08
layout: post
link: http://hugeterry.cn/dreams/597
published: true
tags:
  - 测试
  - 静音
categories:
  - 开发那些事
---
最近公司里边测试组对应用在进行Monkey测试时，因为手机测试而造成太吵，想寻找解决方法，而在Monekey测试时，仅仅把手机调成静音也会被开启。

客户端组想到了如下解决方案，需要写一个服务让手机进入耳机模式：

主要的核心代码：

<pre><code>audioManager.setSpeakerphoneOn(false);
if (Build.VERSION.SDK_INT &gt;= Build.VERSION_CODES.HONEYCOMB){
audioManager.setMode(AudioManager.MODE_IN_COMMUNICATION);
} else {
audioManager.setMode(AudioManager.MODE_IN_CALL);
}</code></pre>

在服务里启动音频管理器AudioManager对其修改即可

使用服务启动它后，设置对应的action，用adb am命令启动它即可

<pre><code>adb shell am startservice -a &lt;对应的action&gt; -n &lt;包名&gt;/&lt;服务名&gt;</code></pre>

如：
<pre><code>adb shell am startservice -a cn.hugeterry.stoptalk.ACTION_START -n cn.hugeterry.stoptalk/.StopTalkingService
</code></pre>
&nbsp;