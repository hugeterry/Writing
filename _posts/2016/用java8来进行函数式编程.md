---
ID: 347
post_title: 用Java8来进行函数式编程
post_name: '%e7%94%a8java8%e6%9d%a5%e8%bf%9b%e8%a1%8c%e5%87%bd%e6%95%b0%e5%bc%8f%e7%bc%96%e7%a8%8b'
author: HugeTerry
post_date: 2016-03-20 00:53:32
layout: post
link: http://hugeterry.cn/dreams/347
published: true
tags:
  - Java
  - Lambda
  - 函数式编程
  - 面向接口编程
categories:
  - 学习总结
---
<blockquote>
<h3> 这个标题有点意思</h3>
</blockquote>
<div>Java8的明显特点是通过默认接口定义、Lambda表达式、方法引用、函数式接口带来了函数式编程，这些功能的出现也改变了java多年来的一些习惯</div>
&nbsp;
<h1>接口定义增强：</h1>
<blockquote>
<h3>这是一个极其毁三观的方式</h3>
</blockquote>
java的接口一直是由全局常量和抽象方法组成，但是在Java8出现后，这一个形势就因此改变了...

场景：存在一个接口，而同时有2k个类实现了该接口，突然有一天需求更改，需在接口里添加一个方法，而所有实现该接口的子类该方法的实现是一样的，按照之前的方式，需在每一个子类复写该方法，so....你需要复制粘贴2k次
<blockquote>
<h3>为了解决这个问题，default就诞生了</h3>
</blockquote>
<div>default示例
<!--?prettify linenums=true?--></div>
<pre class="prettyprint">interface Formula {
    double calculate(int a);
    default double sqrt(int a) {
        return Math.sqrt(a);
    }
    //static方式
    static void get（）{
         system.out.println("...");
    }
}
Formula formula = new Formula() {
    @Override
    public double calculate(int a) {
        return sqrt(a * 100);
    }
};

formula.calculate(100);     // 100.0
formula.sqrt(16);           // 4.0
//static方式
Formula.get();</pre>
<br clear="none" />除了用default定义方法，一旦使用了static定义方法意味着这个方法只可以直接由类名称调用。
<div>另外还有一个重要概念：<span style="text-decoration: underline;">内部类访问方法参数的时候可以不加上final关键字</span></div>
&nbsp;
<h1> Lambda表达式</h1>
<div>lambda属于函数式编程的概念</div>
<div>传统的匿名内部类，Android中添加监听器的典型例子</div>
<div>
<pre class="prettyprint">Button button = (Button) findViewById(R.id.button);
button.setOnClickListener(new OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(MainActivity.this, "Button Clicked", Toast.LENGTH_SHORT).show();
    }
});</pre>
<blockquote>
<h3>这段代码好繁琐</h3>
</blockquote>
这个代码认真一看，其实主要运用到的代码仅仅只有Toast使用的这一句，但由于java的面向对象语法，不得不嵌套更多内容

做法太过严谨，于是java8引入了函数式编程简化语法
<blockquote>
<h3>怎么简化呢？</h3>
</blockquote>
Lambda范例：
<pre class="prettyprint">Button button = (Button) findViewById(R.id.button);
button.setOnClickListener(v-&gt;Toast.makeText(MainActivity.this, "Button Clicked", Toast.LENGTH_SHORT).show());</pre>
长度减了一大半，使用了Lambda表达式大大简化了语法
<blockquote>
<h3>道理我都懂，怎么使用？</h3>
</blockquote>
<h2>Lambda语法三种形式</h2>
<ul>
 	<li>（参数）-&gt;单行语句；    () -&gt; System.out.println("hello")</li>
 	<li>（参数）-&gt;{单行语句}； (String s) -&gt; { System.out.println(s); }</li>
 	<li>（参数）-&gt;表达式     (int x, int y) -&gt; x + y</li>
</ul>
范例：
<pre class="prettyprint">  public void runnableTest() {
        // 一个匿名的 Runnable
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello world one!");
            }
        };

        // Lambda Runnable
        Runnable r2 = () -&gt; System.out.println("Hello world two!");
        // 执行两个 run 函数
        r1.run();
        r2.run();
    }</pre>
<blockquote>
<h3>让我再举一个简化<span style="text-decoration: underline;">变得更短</span>的例子</h3>
</blockquote>
<pre class="prettyprint">List&lt;String&gt; names = Arrays.asList("peter", "anna", "mike", "xenia");
Collections.sort(names, (String a, String b) -&gt; b.compareTo(a));
//让他再变得更短些
Collections.sort(names, (a, b) -&gt; b.compareTo(a));</pre>
<blockquote>
<h3>嘿嘿，看明白了吗</h3>
</blockquote>
<span style="text-decoration: underline;">当只有一个表达式时，那么会直接进行返回操作</span>

&nbsp;
<h1> 方法引用</h1>
以前更多的是在对象上使用引用，而java8多出的是方法引用
<blockquote>
<h3>这是什么鬼？</h3>
<h3>待会再跟你解释.</h3>
</blockquote>
方法引用需要使用 :: 关键字，这是java8才有的东东

接下来，让我们说说四种形式方法引用：

</div>
<div>
<ul>
 	<li>引用静态方法：类名称 :: static 方法名称；</li>
 	<li>引用某个对象的方法：实例化对象 :: 普通方法；</li>
 	<li>引用特定类型的方法：特定类 :: 普通方法</li>
 	<li>引用构造方法：类名称 :: new</li>
</ul>
例子：引用静态方法：

在String类里面有一个valueOf()方法：public static String valueOf（int x）;
<pre class="prettyprint">interface Inter&lt;P,R&gt;{
     public R zhuanhuan(P p);
}
public class Test{
     public static void main(String args[]){
         Inter&lt;Integer,String&gt; msg = String::valueOf;
         String str = msg.zhuanhuan(3000);
         System.out.println(str); // 3000
         //原始方法
         Inter&lt;Integer, String&gt; msg = new Inter&lt;Integer, String&gt;() {
		public String zhuanhuan(Integer p) {
			return  String.valueOf(p);
		}
	};
         //lambda 
         Inter&lt;Integer, String&gt; msg =（p）-&gt;String.valueOf(p);
    }
}</pre>
通过
<pre class="prettyprint">Inter&lt;Integer,String&gt; msg = String::valueOf;</pre>
让Inter的R方法拥有了valueOf的功能
<blockquote>
<h3>卧槽，这....不就是传说中的直接<span style="text-decoration: underline;">复制敌人绝招</span>嘛，</h3>
</blockquote>
将String.valueOf()方法变为了Inter接口里的R()方法,

再来另外三个例子看看？

例子：引用普通方法：
<pre class="prettyprint">@FunctionalInterface //此为函数式接口，只能定义一个方法
interface Inter&lt;P,R&gt;{
     public R upper();
     //<del>public void print();</del>
}
public class Test{
     public static void main(String args[]){
          //要在实例化对象下使用
          //hello是String的实例化对象
          Inter&lt;Integer,String&gt; msg = "hello"::toUpperCase;
          String str = msg.Upper();
          System.out.println(str); // HELLO
     }
}</pre>
此时我们应该有了疑问：

通过两个代码演示，如果要实现函数引用，接口里必须只存在一个方法。如果再来一个方法，方法不是无法引用了吗？如划线语句
<pre class="prettyprint">    //<del> public void print();</del></pre>
所以为了保证被引用接口里面只有一个代码，需加上注解@FunctionalInterface 此为函数式接口

之前引用的方式中，都是静态方法，
<blockquote>
<h3>接下来我们试试引用普通方法需实例化</h3>
</blockquote>
例子：引用特定类方法 ,比较方法public int compareTo(String anotherString);

</div>
<div>
<pre class="prettyprint">interface Inter&lt;P&gt;{
     public int compare(P p1,P p2);
}
public class Test{
     public static void main(String args[]){
          Inter&lt;String&gt; msg = String::compareTo;
          System.out.println(msg.compare("A","B")); // -1
     }
}</pre>
</div>
<div>与之前相比，方法引用前不再需要定义对象，而是可以理解为将对象定义在了参数上<br clear="none" /><br clear="none" /></div>
<div>例子：引用构造方法</div>
<blockquote>
<h3>又一个毁三观的功能</h3>
</blockquote>
<pre class="prettyprint">interface Inter&lt;C&gt;{
     public C create(String t,double p);
}
class Book {
     private String title;
     private double price;
     public Book(String title,double price){
         this.title = title;
         this.price = price;
     }
     public String toString(){
         return "book name:"+this.title+",book price:"+this.price;
     }
}
public class Test{
     public static void main(String args[]){
         Inter&lt;Book&gt; msg = Book::new;
         Book book = msg.create("java",20);
         System.out.println(book);//book name:java,book price:20
     }
}</pre>
那为啥java8不定义这些接口直接给我们使用呢？
<blockquote>
<h3>当然有啦</h3>
</blockquote>
&nbsp;
<h1> 函数式接口</h1>
jdk8提供了一个函数式接口包java.util.function,里面有众多的函数式接口，而其中最基础最常操作的有以下四个核心接口：

功能型接口：
<ul>
 	<li>public interface Function&lt;T,R&gt;{public R apply(T t);}</li>
 	<li>接收一个参数 返回一个结果</li>
 	<li>例如String.valueOf()</li>
</ul>
消费型接口：
<ul>
 	<li>public interface Consumer&lt;T&gt;{public void accept(T t);}</li>
 	<li>接收参数 不返回结果</li>
 	<li>例如System.out.println</li>
</ul>
供给型接口：
<ul>
 	<li>public interface Supplier&lt;T&gt;{public void get(T t);}</li>
 	<li>不接收参数 返回结果</li>
 	<li>例如String的toUpperCase()</li>
</ul>
断言型接口：
<ul>
 	<li>public interface Predicate&lt;T&gt;{public boolean test(T t);}</li>
 	<li>判断操作使用</li>
 	<li>例如String的equalsIgnoreCase()</li>
</ul>
<blockquote>
<h3><span style="font-size: 14px;">说这些，来个例子？ </span></h3>
</blockquote>
<pre class="prettyprint">public class Test{
     public static void main(String args[]){
          //功能型接口
          Function&lt;String,Boolean&gt; fun = "hello"::startsWith;
          System.out.println(fun.apply("he"));          //true
          //消费型接口
          Consumer&lt;String&gt; cons = System.out::println;
          cons.accept("hello");                         //hello
          //供给型接口
          Supplier&lt;Stirng&gt; sup = "hello"::toUpperCase;
          System.out.println(sup.get());                //HELLO
          //断言型接口
          Predicate&lt;String&gt; pre ="hello"::equalsIgnoreCase;
          System.out.println(pre.test("Hello"));        //true    
          }
}</pre>
这几个接口包含的各种引用，也是函数式接口的代表，那么存在其他的众多接口中，都是这四个接口的扩展提升

&nbsp;

So，这些就是Java8带来的新特性啦
<blockquote>
<h3>多多实践有利掌握</h3>
</blockquote>
&nbsp;

<hr />

<h2></h2>
<h2>题外话</h2>
听说Android sdk23.2.0也支持Java8了
<blockquote>那还没更新的呢？！

莫慌....</blockquote>
你可以使用 <a href="https://github.com/evant/gradle-retrolambda" target="_blank">gradle-retrolambda</a> 插件把 Lambda 表达式 转换为 <span class="highlight">Java</span> 7 版本的代码。试试你就知道啦哈哈

&nbsp;