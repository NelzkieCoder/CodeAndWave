---
title: Java - Interface
lang: en
category: articles
layout: post
tagline: Abstraction with Interface
image: "/assets/img/profile-small.png"
header:
  image: "/assets/img/profile-small.png"
tags:
- Java
- OOP
keywords: Java, Fundamentals, This, This Keyword
date: '2017-11-25 08:13:28 +0000'
---

Java Interface is bit like [abstract class]({% post_url 2017-11-15-Java-Abstraction%}) but with the exception that an interface **can only** have a method declaration and fields. Nothing more nothing less.  The definition of methods will be on the classes that implemented the interface. While abstract classes can have a concrete method inside it, an interface cannot. It is also worth noting that methods declared on interfaces are always abstract. 

<!--break-->

### Why use Interface?
* Use in abstraction
* Grants multiple [inheritance]({% post_url 2017-11-11-Java-Inheritance%})
* Can be use in implementing callbacks
* Use in many design patterns like MVP(Model View Presenter) in android
* Can be use to achieve loose coupling
* Can be use in **composition** ( has-a relationship)

### When and How to use interface?

<p><span class="warning"> <span style="font-weight:bold;">NOTE:</span> For interface to make sense, you must understand how <a href = "{{% post_url 2017-11-12-Java-This-Keyword %}}">abstract class</a> work and its limitations</span></p>

Continuing our story from [abstract class]({% post_url 2017-11-15-Java-Abstraction%}) where we left it, since we can only extend to one class and we don't want our cat and dog to have a CanFly() method but only the bird class, what we could do is to make an interface called
CanFly with an abstract method called Fly().

``` java
public interface CanFly {
      void fly();    // this method is by default an abstract
}
```

And then implement this interface on our Bird class

``` java
public class Bird implements CanFly {
    @Override
    public void fly() {
        System.out.println("Birds can fly");
    }
}
```

Obviously we are missing some things here. Can't the bird sleep, eat and reproduce? Ofcourse they can! We don't have it here because we didn't **extend** to our animal(our base) class.
``` java
public class Bird extends Animal implements CanFly {
    @Override
    public void fly() {
        System.out.println("Birds can fly");
    }

    @Override
    protected void MakeNoise() {
        System.out.println("tweet tweet tweet ");
    }
}
```
 We also implemented the MakeNoise() on our bird class  with its own definition. If we compare this to our cat and dog class it would look like this
 
 ``` java
 
 public class Cat extends Animal {
    public Cat() {  this.name = "Cat"; }
		
    @Override
    protected void MakeNoise() { System.out.println("Meow Meow"); }
}
public class Dog extends Animal {
    public Dog() { this.name = "Dog"; }
		
    @Override
    protected void MakeNoise() {  System.out.println("Woof Woof");  }
}
 
 public class Bird extends Animal implements CanFly {  // Only bird have the CanFly interface implemented

    public Bird() { this.name = "Bird"; }
    
    @Override
    protected void MakeNoise() { System.out.println("tweet tweet tweet "); }
		
    @Override
    public void fly() {
        System.out.println("Birds can fly");    // The fly() definition
    }

}
   public static void main(String[] args) {

        Dog dog = new Dog();
        dog.CanEat();
        dog.CanSleep();
        dog.CanReproduce();
        dog.MakeNoise();

        Cat cat = new Cat();
        cat.CanEat();
        cat.CanSleep();
        cat.CanReproduce();
        cat.MakeNoise();
        
        Bird bird = new Bird();
        bird.MakeNoise();
        bird.CanEat();
        bird.CanReproduce();
        bird.CanEat();
        bird.fly();  // BAAM! birds can fly
      
    }
```

Hopefully this code makes sense. The code is self explanatory. If you still don't understand why, you might want to code this to your IDE and then  run it.

### Achieving Multiple inheritance
Now supposed your other angel( cause you're a god, you got lots of angels) wants to make a pegasus(a mythical creature in greek mythology). A pegasus can fly and  has a horn  and has the traits of any other animals like eating and sleeping. The only thing we are missing here then is the horn. So let's make an interface for that.

``` java
public interface HasHorn {
    void OneHorn();
}
```

Our pegasus class would now look like this:
``` java
public class Pegasus extends Animal implements HasHorn, CanFly {
    public Pegasus() {
        this.name = "Pegasus";
    }

    @Override
    protected void MakeNoise() {
        System.out.println("A pegasus sound maybe? who knows");
    }

    @Override
    public void fly() {
        System.out.println("pegasus can fly");
    }

    @Override
    public void OneHorn() {
        System.out.println("Horn on its forehead");
    }
}
```

See that we implemented the HasHorn and CanFly? Pretty cool right. This is what **multiple inheritance** looks like. You can add as many interface as you want. If you want a pegasus that can breath fire just make an interface of it and then implement it. 


### Conclusion
If we want to be better at designing our app or system  and make our objects **loosely coupled** it is important to understand the use of interface.  Hopefully this article shed you some light about  how interface works. You can keep on coding  without using interface but as soon as your codebase becomes really large, you will find that it is very diffcult to maintain or change it because it is **tightly coupled**. While it might be confusing at first, keep in mind that it takes time to fully understand how interface works. But as soon as you do, you are going to love it.  Here is the [source code](https://github.com/NelzkieCoder/CodeAndWave_Topic_Source/tree/master/Java/Abstraction/src). Till next time, HAPPY CODING!