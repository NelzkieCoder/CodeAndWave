---
layout: post
title: Java - The this Keyword
tagline: "Not this again!"
image: /assets/img/profile-small.png
header:
  image: /assets/img/profile-small.png
tags: ["Java","Fundamentals"]
keywords: Java, Fundamentals, This, This Keyword
lang: en
date: 2017-11-12T08:13:28+00:00
category: articles
---

The **this** keyword can be very confusing for beginners because we don't know what is the **this** keyword referring to. In this topic we'll dicuss the use of **this** keyword in Java.

<!--break-->
### The uses of **this** keyword
* **It can be use to refer to the current instance class variables**

{% highlight java %}
public class MyClassOne {
    String name;
    public MyClassOne(String name) {
        this.name = name; // the this.name refer to the name variable and not the name argument.
        name = name; // not gonna work, you are passing the argument name to itself.
    }
}
{% endhighlight %}
{% highlight java %}
Output:
CodeAndWave
{% endhighlight %}

<p>In the first line of the code above, the <b>this</b> keyword in the <b>this.name</b> refers to the instance variable and <b>not</b> the name argument.</p>
In the second line, it is not gonna work because you are passing the name parameter to itself.
<br />
<br />
* **It can be used to call current class constructor**.
<p>We can do this by using <span style="color:#e4a048;">this()</span></p>
{% highlight java %}
public class MyClassTwo {

    String name;

    public MyClassTwo() {
        this("CodeAndWave - From default Constructor");
    }

    public MyClassTwo(String name) {
        this.name = name;
    }

    void print(){
        System.out.println(name);
    }
}
{% endhighlight %}
<p><span class="warning"> <span style="font-weight:bold;">NOTE:</span> The Call to this() must be the first statement in constructor. Meaning if you call another this() with different signature the compiler won't allow you.</span></p>

<p>
    See the constructor with no argument? We call the <b>this</b> keyword and then <b>pass</b> a string argument. The compiler then will find a constructor that <b>matches</b> the signature and then call that constructor.
</p>

<br />


* **It can be pass as an argument to a method**
{% highlight java %}
public class MyClassOne {

    String name;

    public MyClassOne(String name) {
        this.name = name; // the this.name refer to the name variable and not the name argument
    }

    void print(){
        System.out.println(name);
    }

    public void printFromMyClassThree(){
        MyClassThree myClassThree = new MyClassThree();
        myClassThree.printMyClassOne(this); // see the 'this' keyword here? We use the it to pass the instance of the current class
                                            // the 'this' keyword here is referring to the MyClassOne object
    }
}

public class MyClassThree {

    public void printMyClassOne(MyClassOne myClassOne){
        myClassOne.print(); // see this? we call the print() function of MyClassOne
    }
}

public class Main {

    public static void main(String[] args) {
        MyClassOne myClassOne = new MyClassOne("CodeAndWave");
        myClassOne.printFromMyClassThree();
    }
}

{% endhighlight %}
{% highlight java %}
Output:
CodeAndWave
{% endhighlight %}

<p>Take a look at the code carefully, read it, and try it on your IDE. This is the only way you could understand what is happening here. Like I said, the <b>this</b> keyword can be confusing sometimes.
</p>
<br />

* **It can be use to return class instance state**
{% highlight java %}
public class Person {
        String name;
        int age;
    public Person() {
        this("CodeAndWave",20); // the this here is referring to the constructor that matches its signature
    }
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    Person getPersonInfo() {

        return  this; // the this here is referring to current instance class
     }

}

public class Main {
    
    public static void main(String[] args) {
        Person p = new Person();
        System.out.println("Hi I am "+p.getPersonInfo().name + "  and my age is " + p.getPersonInfo().age);
    }
}

{% endhighlight %}

{% highlight java %}
Output:
Hi I am CodeAndWave  and my age is 20
{% endhighlight %}
<p>
<b>Take your time and read the code carefully</b> cause it is a bit confusing.</p> <p>The first <b>this</b> on the default constructor
{% highlight java %}
this("CodeAndWave",20);
{% endhighlight %}

is <span style="color:#e4a048;">referring</span> to the constructor that matches its signature(the <span style="color:#e4a048;">second</span> constructor).
</p>

<p>
The second <b>this</b> keyword
{% highlight java %}
Person getPersonInfo() {
        return  this; // the this here is referring to current instance class
     }
{% endhighlight %}
is referring to the <span style="color:#e4a048;">current object</span>(the <span style="color:#e4a048;">current instance</span> class)
</p>

<br />
<p>

Hopefully you understand even just a bit about how the <b>this</b> keyword works. I know it is confusing so just take your time. Try it on your IDE. Run it and mess with it. Thats the best way to understand this <b>this</b> keyword. HA! See what I did there? this "this".

</p>