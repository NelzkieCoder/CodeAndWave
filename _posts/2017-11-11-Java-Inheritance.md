---
layout: post
title: Java OOP - Inheritance
tagline: "The First Pillar of OOP"
image: https://postimg.org/image/vefxherup/
feature-img: "assets/img/unsplash/coding.jpg"
tags: ["Java","OOP"]
keywords: Java, OOP, Inheritance
lang: en
date: 2017-11-11T08:13:28+00:00
category: articles
---

Inheritance is one of the 4 pillars of Object Oriented Programming commonly known as **'OOP'**

### What is inheritance?
Inheritance is a concept where one class acquire all the properities and behaviors of another class.
It  is usually represented as a **<span style="color:#e4a048;">Is - a</span>** relationship.

<!--break-->

#### Example
* A dog  ***<span style="color:#e4a048;">is an </span>*** animal
* An apple ***<span style="color:#e4a048;">is a </span>*** fruit

### Why use inheritance?
* Inheritance is used to achieve **<span style="color:#e4a048;">polymorphism</span>**(we'll takcle this on future topic)
* It is also used to remove redudant code for traits that are the same as with other classes.

#### Example Scenario
Suppose you are a so called "god" and want to create a dog and a cat. Both can eat, sleep, reproduce. These are traits that are needed for survival. So you sat down on your awesome chair and started coding which would look like this:

  
{% highlight java %}
public class Dog {

    private void CanEat(){
        System.out.println("Dog can eat");
    }
    private void CanSleep(){
        System.out.println("Dog can sleep");
    }
    private void CanReproduce(){
        System.out.println("Dog can reproduce");
    }
}


public class Cat {
  
    private void CanEat(){
        System.out.println("Cat can eat");
    }
    private void CanSleep(){
        System.out.println("Cat can sleep");
    }
    private void CanReproduce(){
        System.out.println("Cat can reproduce");
    }
}

{% endhighlight %}

Looks easy right? Now what if you decided to make more? Maybe a **hundred**? Will you keep on typing the same thing over and over? Typing the same function or copy pasting the same function to another class? Ofcourse not. What we have to do is think of a better way to lessen our work. Can you see the **common traits** here? The **dog** and **cat** share the same survival trait and they're both animals. What if we create an **Animal** class, put all the function to that class and then make the **dog** and the **cat** class derived from it? Let's see:

{% highlight java %}

public class Animal {

    public String name = "Dog";

    protected void CanEat(){
        System.out.println( name + " can eat");
    }
    protected void CanSleep(){
        System.out.println(name + " can sleep");
    }
    protected void CanReproduce(){
        System.out.println(name + " can reproduce");
    }
}

public class Cat extends Animal {
    public Cat() {
        this.name = "Cat";
    }
}

public class Dog extends Animal {
    public Dog() {
        this.name = "Dog";
    }
}


public static void main(String[] args) {

        Dog dog = new Dog();
        dog.CanEat();
        dog.CanSleep();
        dog.CanReproduce();


        Cat cat = new Cat();
        cat.CanEat();
        cat.CanSleep();
        cat.CanReproduce();
    }

{% endhighlight %}
{% highlight java %}
Output:
Dog can eat
Dog can sleep
Dog can reproduce
Cat can eat
Cat can sleep
Cat can reproduce

{% endhighlight %}

Awesome isn't. Now, even though the **dog** and **cat** class doesn't have any functions in them, their **<span style="color:#e4a048;">base</span>**(or also called as **'<span style="color:#e4a048;">super</span>'**) class which is the **animal** does. By <span style="color:#e4a048;">default</span> when you call for example the **CanEat()** fucntion like this:

{% highlight java %}
 Cat cat = new Cat();
cat.CanEat();
Dog dog = new Dog();
dog.CanEat();
{% endhighlight %}
 you are calling a function on the <span style="color:#e4a048;">derived</span>(the **<span style="color:#e4a048;">dog</span>** and the **<span style="color:#e4a048;">cat</span>**) class that look like this
{% highlight java %}
 @Override
    protected void CanEat() {
        super.CanEat();
    }
{% endhighlight %}

See the  **<span style="color:#e4a048;">super.CanEat()</span>**? That is our derived class **calling** the base class function. That is why the output shows that the dog and cat can eat,sleep, and reproduce.

But what if you decided that cats can no longer eat? Easy just **<span style="color:#e4a048;">override</span>** the function and remove the **<span style="color:#e4a048;">super.CanEat()</span>** function like this:
{% highlight java %}
 @Override
    protected void CanEat() {
          System.out.println("No! cat can't eat now, Im evil!");
    }
{% endhighlight %}

The  <span style="color:#e4a048;">@Override</span> keyword is important to indicate that you want to **<span style="color:#e4a048;">own</span>** the function and not just inherit from the base class.

 You can also see that the **name** variable was called using the **this** keyword which was declared on the base class. You could also call the keyword **super** if you want instead of calling the **this** keyword
{% highlight java %}
this.name = "dog"
{% endhighlight %}
is the same as
{% highlight java %}
super.name = "dog"
{% endhighlight %}

<p><span class="warning"> <span style="font-weight:bold;">NOTE:</span> The <b>this</b> keyword can be a bit confusing cause it not only points to the base class, it can also points to functions and the class itself</span></p>

If you want to know more about the <span style="color:#e4a048;"> this</span> keyword click [here]({{ site.baseurl }}{% post_url 2017-11-12-Java-This-Keyword %})



## Conclusion

Hopefully you gain a bit of understanding of how inheritance work. Remember to design your system well, if all functions share the same traits put them on the base class. Don't put anything in the base class that only one or two of your derived classes will use, this will lead to **tight coupling** if you are not careful. 