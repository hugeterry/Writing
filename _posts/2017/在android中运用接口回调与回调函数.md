---
ID: 540
post_title: >
  在Android中运用接口回调与回调函数
post_name: '%e5%9c%a8android%e4%b8%ad%e8%bf%90%e7%94%a8%e6%8e%a5%e5%8f%a3%e5%9b%9e%e8%b0%83%e4%b8%8e%e5%9b%9e%e8%b0%83%e5%87%bd%e6%95%b0'
author: HugeTerry
post_date: 2017-01-18 13:17:20
layout: post
link: http://hugeterry.cn/dreams/540
published: true
tags:
  - Android
  - Java
  - 回调函数
  - 面向接口编程
categories:
  - 开发那些事
---
最近在啃设计模式，所以会注意到一些架构化的设计，接口回调与回调函数也是在项目中结合网上的一些Java接口回调与回调函数的文章抽出来总结的。

&nbsp;
<h2>接口回调</h2>
关于接口回调，这是网上一篇<a href="http://www.blogjava.net/Carter0618/archive/2007/08/19/137936.html">关于Java接口回调与回调函数文章</a>的概念以及代码示例：
<blockquote>接口回调是指：可以把使用某一接口的类创建的对象的引用赋给该接口声明的接口变量，那么该接口变量就可以调用被类实现的接口的方法。实际上，当接口变量调用被类实现的接口中的方法时，就是通知相应的对象调用接口的方法，这一过程称为对象功能的接口回调。
<pre><code>interface People{
   void peopleList();
}
class Student implements People{
   public void peopleList(){
      System.out.println("I’m a student.")；
   }
}
class Teacher implements People{
   public void peopleList(){
      System.out.println("I’m a teacher.");
   }
}
public class Example{
   public static void main(String args[]){
       People a;             //声明接口变量
      a=new Student();      //实例化，接口变量中存放对象的引用
      a.peopleList();        //接口回调
      a=new Teacher();     //实例化，接口变量中存放对象的引用
      a.peopleList();       //接口回调
   }
}</code></pre>
</blockquote>
有点抽象？其实直接这么理解就行了：
<blockquote>接口  a =new 实例化接口的对象()；</blockquote>
&nbsp;

这在设计模式中也是经常用到的，这也是面向对象的一大精髓 -&gt; 面向接口的一个做法，面向接口能够给代码带来更多的解耦，结合设计模式使用能发挥更大作用，面向接口的好处在这里也不多详述了。

&nbsp;
<h2>回调函数</h2>
回调函数的过程如图所示，<img class="size-full alignright" src="http://7xsyv9.com2.z0.glb.qiniucdn.com/callback.JPG" width="341" height="198" />

关于回调函数，有个<a href="http://blog.csdn.net/xiaanming/article/details/17483273">不错的例子</a>，摘自里面的一段解释：
<blockquote>所谓回调：就是A类中调用B类中的某个方法C，然后B类中反过来调用A类中的方法D，D这个方法就叫回调方法</blockquote>
在Android里边比较不错的例子是
<pre><code>mButton.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        //TODO:events
    }
});
</code></pre>
其中，<code>onClick(View view)</code>就是回调函数，这个流程是这样的：

在当前类中调用了View类的<code>setOnClickListener()</code>方法，而View类反过来调用了当前类的<code>onClick(View view)</code>

原理就是这样，至于Android源码是怎么回调这个<code>onClick(View view)</code>的，可以去看看Android的官方源码

&nbsp;
<h2>在Android中用接口实现回调函数的等价功能</h2>
这是我在使用recycleview中，加载更多时用到的。需求是这样的，在主界面<a href="http://www.hugeterry.cn/wp-content/uploads/2017/01/QQ20170118-0.png"><img class="alignnone wp-image-545" src="http://www.hugeterry.cn/wp-content/uploads/2017/01/QQ20170118-0.png" alt="" width="275" height="421" /></a> <a href="http://www.hugeterry.cn/wp-content/uploads/2017/01/QQ20170118-1.png"><img class="alignnone wp-image-546" src="http://www.hugeterry.cn/wp-content/uploads/2017/01/QQ20170118-1.png" alt="" width="275" height="422" /></a>

底部有个点击重新加载之后变为第二张图，那么怎么做的呢，结合回调接口，首先写好一个接口类
<pre><code>public interface LoadMoreDataAgainListener {
    void loadMoreDataAgain(TextView textView, View loadingView);
}</code></pre>
接着RecyclerView的Adapter写好创建监听器的方法
<pre><code>public void setOnMoreDataLoadAgainListener(LoadMoreDataAgainListener onMoreDataLoadAgainListener) {
    mLoadMoreDataAgainListener = onMoreDataLoadAgainListener;
}</code></pre>
在RecyclerView.ViewHolder中view的监听器：
<pre><code>@OnClick({R.id.loadingIndicatorView, R.id.tv_load_data_again})
public void onClick(View view) {
    mLoadMoreDataAgainListener.loadMoreDataAgain(mTvLoadDataAgain, mAvLoadingIndicatorView);
}</code></pre>
在fragment类中为adapter传递该监听器：
<pre><code>private AddFooterBaseAdapter mAddFooterBaseAdapter;
mAddFooterBaseAdapter.setOnMoreDataLoadAgainListener(new LoadMoreDataAgainListener() {
    @Override
    public void loadMoreDataAgain(TextView textView, View loadingView) {
        textView.setVisibility(View.GONE);
        loadingView.setVisibility(View.VISIBLE);
        //TODO:加载更多数据
    }
});</code></pre>
至此，一个接口实现回调函数的等价功能就实现了

&nbsp;