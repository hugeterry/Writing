---
ID: 659
post_title: >
  support包中preference下自定义原生alertdialog解决方案
author: hugeterry
post_excerpt: ""
layout: post
permalink: http://hugeterry.cn/dreams/659
published: true
post_date: 2018-02-05 23:08:28
---
<h3>场景</h3>
最近在开发UI控件库中需要对perference自定义样式，而EditTextPreference以及ListPreference等均使用到alertdialog，则需要对alertdialog的样式进行自定义修改

首先从demo出发，Android <a href="https://github.com/aosp-mirror/platform_frameworks_support"> Support包源码</a>中sample并没有对perference作demo，网罗的一些demo，其中不乏两种方式使用support-preference，
<ol>
 	<li><a href="https://github.com/jakobulbrich/preferences-demo">preferences-demo</a></li>
 	<li><a href="https://github.com/aosp-mirror/platform_packages_apps_settings">platform_packages_apps_settings</a></li>
</ol>
<a href="https://github.com/jakobulbrich/preferences-demo">preferences-demo</a>采用的是AppCompatActivity - PreferenceFragmentCompat的搭配，这个时候你要使用appcompat的theme

<a href="https://github.com/aosp-mirror/platform_packages_apps_settings">platform_packages_apps_settings</a>（android8.0 下settings app源码）采用的是Activity-PreferenceFragment的搭配(这个需要仔细去阅读android8.0 settings的源码，在这里不做详述)，可以在activity下使用

使用这两种方式去展示时，会发现Activity-PreferenceFragment的搭配是不能通过在theme下重写alertdialogstyle的样式布局去改变perference控件（例如android.support.v7.preference.EditTextPreference

、android.support.v7.preference.ListPreference）弹出的alertdialog。而AppCompatActivity - PreferenceFragmentCompat是可以的
<blockquote>那么为什么会这样？我们从两者的源码出发去探索</blockquote>
android.support.v7.preference. PreferenceFragmentCompat 源码：

<pre><code>@Override
public void onDisplayPreferenceDialog(Preference preference) {
    .....
    final DialogFragment f;
    if (preference instanceof EditTextPreference) {
        f = EditTextPreferenceDialogFragmentCompat.newInstance(preference.getKey());
    } else if (preference instanceof ListPreference) {
        f = ListPreferenceDialogFragmentCompat.newInstance(preference.getKey());
    } else if (preference instanceof AbstractMultiSelectListPreference) {
        f = MultiSelectListPreferenceDialogFragmentCompat.newInstance(preference.getKey());
    } else {
        throw new IllegalArgumentException("Tried to display dialog for unknown " +
                "preference type. Did you forget to override onDisplayPreferenceDialog()?");
    }
    .....
}</code></pre>

<a href="http://www.hugeterry.cn/wp-content/uploads/2018/02/pref-v7-preference.png"><img class="alignnone  wp-image-660" src="http://www.hugeterry.cn/wp-content/uploads/2018/02/pref-v7-preference.png" alt="" width="540" height="391" /></a>

android.support.v14.preference.PreferenceFragment源码：

<pre><code>@Override
public void onDisplayPreferenceDialog(Preference preference) {
	....
	final DialogFragment f;
	if (preference instanceof EditTextPreference) {
   		f = EditTextPreferenceDialogFragment.newInstance(preference.getKey());
	} else if (preference instanceof ListPreference) {
   		f = ListPreferenceDialogFragment.newInstance(preference.getKey());
	} else if (preference instanceof MultiSelectListPreference) {
    	f = MultiSelectListPreferenceDialogFragment.newInstance(preference.getKey());
	} else {
    	throw new IllegalArgumentException("Tried to display dialog for unknown " +
            	"preference type. Did you forget to override onDisplayPreferenceDialog()?");
	}
	....
}</code></pre>

<a href="http://www.hugeterry.cn/wp-content/uploads/2018/02/pref-v14-preference.png"><img class="alignnone  wp-image-661" src="http://www.hugeterry.cn/wp-content/uploads/2018/02/pref-v14-preference.png" alt="" width="523" height="174" /></a>

即v14包中在PreferenceFragment下使用了EditTextPreferenceDialogFragment、ListPreferenceDialogFragment、MultiSelectListPreferenceDialogFragment，而他们继承了PreferenceDialogFragment

同样的 v7包中在PreferenceFragmentCompat下使用了EditTextPreferenceDialogFragmentCompat、ListPreferenceDialogFragmentCompat、MultiSelectListPreferenceDialogFragmentCompat，而他们继承了PreferenceDialogFragmentCompat。

那么我们接下来看一下 PreferenceDialogFragment 和 PreferenceDialogFragmentCompat 的源码：

android.support.v14.preference. PreferenceDialogFragment 源码：

<pre><code>@Overridepublic @NonNull Dialog onCreateDialog(Bundle savedInstanceState) {
    final Context context = getActivity();
    mWhichButtonClicked = DialogInterface.BUTTON_NEGATIVE;

    final android.app.AlertDialog.Builder builder = new AlertDialog.Builder(context)
            .setTitle(mDialogTitle)
            .setIcon(mDialogIcon)
            .setPositiveButton(mPositiveButtonText, this)
            .setNegativeButton(mNegativeButtonText, this);
	//...省略
    return dialog;
}</code></pre>

&nbsp;

android.support.v7.preference.PreferenceDialogFragmentCompat源码：

<pre><code>@Override
public @NonNull
Dialog onCreateDialog(Bundle savedInstanceState) {
    final Context context = getActivity();
    mWhichButtonClicked = DialogInterface.BUTTON_NEGATIVE;

    final android.support.v7.app.AlertDialog.Builder builder = new AlertDialog.Builder(context)
            .setTitle(mDialogTitle)
            .setIcon(mDialogIcon)
            .setPositiveButton(mPositiveButtonText, this)
            .setNegativeButton(mNegativeButtonText, this);

}</code></pre>

v14下的PreferenceDialogFragment 使用的是android.app.AlertDialog，而v7下的PreferenceDialogFragmentCompat使用的是android.support.v7.app.AlertDialog

那么，接下来问题便转化成：
<blockquote>原生AlertDialog和v7下的AlertDialog有什么不同</blockquote>
我们知道，AlertDialog的源码使用了建造者模式，用到了AlertController去进行控制

com.android.internal.app. AlertController源码：

<pre><code>protected AlertController(Context context, DialogInterface di, Window window) {
    ...
    final TypedArray a = context.obtainStyledAttributes(null, R.styleable.AlertDialog,                				com.android.internal.R.attr.alertDialogStyle, 0);
    ...
}</code></pre>

&nbsp;

android.support.v7.app. AlertController源码:

<pre><code>public AlertController(Context context, AppCompatDialog di, Window window) {
	...
    final TypedArray a = context.obtainStyledAttributes(null, R.styleable.AlertDialog,
                android.support.v7.appcompat.R.attr.alertDialogStyle, 0);
	...
}</code></pre>

于是乎比较原生和v7下的AlertController会发现原生使用的是com.android.internal.R.styleable.AlertDialog，我们是无法通过更改alertdialogstyle去修改原生的样式的，虽然官方在官方文档中有提供如下api
<blockquote><a href="https://developer.android.com/reference/android/app/AlertDialog.Builder.html#AlertDialog.Builder(android.content.Context,%20int)">AlertDialog.Builder</a>(<a href="https://developer.android.com/reference/android/content/Context.html">Context</a> context, int themeResId)

Creates a builder for an alert dialog that uses an explicit theme resource.</blockquote>
但是AlertDialog在v14中是<strong>以方法的局部变量</strong>使用的，这就导致了v14 PreferenceDialogFragment下使用的AlertDialog是无法通过在theme下重写alertdialogstyle的样式布局去改变perference控件样式布局。而v7使用的AlertDialog是可以通过在theme下重写alertdialogstyle这个style改变其样式以及布局的。

&nbsp;

梳理一下

也即是说，<strong>在</strong>android.support.v14.preference.<strong>PreferenceFragment使用preference控件，弹出的AlertDialog会使用原生的样式以及布局，这是无法通过调用api改变的</strong>，而PreferenceFragment继承了android.app.Fragment，可以在Activity下使用，所以如果你在Activity-PreferenceFragment这套方案下使用时，无法改变AlertDialog的样式。

在v7的PreferenceFragmentCompat使用preference控件，弹出的AlertDialog是可以通过在theme下重写alertdialogstyle的样式布局去改变改变的
<h3>解决方案</h3>
那么接下来，为了适配更多的方式，我需要在android.support.v14.preference.PreferenceFragment弹出的AlertDialog去改变其样式和布局，想到了两种策略：
<ol>
 	<li>hook注入，偷梁换柱</li>
 	<li>重新写一套v14下的PreferenceFragment，包括android.app.AlertDialog，改为自己使用的样式（其实在UI库中已经将android.app.AlertDialog重写了，所以这套方案那没有想象之中的困难）</li>
</ol>
api-hook这套方案需要用反射进去对android.app.AlertDialog进行修改

hook点如果是android.app.AlertDialog那么对整个R文件需要修改，工程量很大。

退一步，假如在android.support.v14.preference.PreferenceDialogFragment 的onCreateDialog（Bundle savedInstanceState）进行修改，那么其中AlertDialog为方法的局部变量，也没有办法反射，需要直接拿到onCreateDialog下的所有变量，对整个方法进行偷梁换柱，这种方法可行，但工程量较大。

接下来想到了还有aop进行hook注入，同样工作量不小。

而且如果使用hook的话，那么假如app接入了UI库运行在各种Android手机上，各种手机会对系统源码做定制，假如修改了android.app.AlertDialog的代码，而又对他进行hook时，这样就不安全了。

于是最后的方案选择了第二个方案：重新写一套v14下的PreferenceFragment

&nbsp;

这次的思路应该一路下来看源码和做修改没有大差错，好处是自己熟悉系统以及兼容包源码中preference和alertdialog下配合使用的部分，也了解hook的一些局限性和导致的后果

附，参考文章：

android preference：
<p class="postTitle"><a id="cb_post_title_url" class="postTitle2" href="http://www.cnblogs.com/valenhua/p/7624640.html">Android Preference 设置偏好全攻略</a></p>
hook：
<p class="post-title"><a href="http://weishu.me/2016/01/28/understand-plugin-framework-proxy-hook/">Android插件化原理解析——Hook机制之动态代理</a></p>
<p class="title"><a href="https://www.jianshu.com/p/4f6d20076922">理解 Android Hook 技术以及简单实战</a></p>
&nbsp;