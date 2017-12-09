---
title: Java - Abstraction
lang: en
category: articles
layout: post
tagline: Second pillar of OOP
image: "/assets/img/profile-small.png"
header:
  image: "/assets/img/profile-small.png"
tags:
- Java
- OOP
keywords: Java, OOP, Abstraction
date: '2017-11-15 08:13:28 +0000'
---

Abstraction is a way of well making a certain object abstract. It is a way of **hiding the implementation** to the user. Abstract classes also can't be instantiated but can only be subclassed(derived from it). Meaning you can't do it like this:

<!--break-->

`       Cat a = new Animal();  // Wrong`



There are two ways to achieve abstraction in java. It is using:
* **Abstract class**
* **[Interface]({% post_url 2017-11-25-java-interface %})**

### Let's start with Abstract class
To make an abstract class one must use the keyword **abstract** when declaring a class we do it like this:
``` java
public abstract class Animal {
    public String name = "Animal";

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
```
There isn't much difference to our [inherticane]({% post_url 2017-11-11-Java-Inheritance %}) code. We only add the keyword **abstract** here. But with abstract class also comes **abstract method.** In our example, every animal make different noises. Cat "meows" and the dog "woofs". Ofcourse these implementation are specific  only for derived classes of animal class. We also want  to make sure that if one of our angels(keep in mind you are a so called "god" in our story here) decided to make their own animal we want them to make a noise as well. So we make this MakeNoise() a **REQUIREMENT** when creating an animal. How do we do it? Well we use **abstract** method.

`  protected abstract void MakeNoise();`

We declare this on our animal class(our **base** class). So our base class would look like this

``` java
public abstract class Animal {

    public String name = "Animal";

    protected void CanEat(){
        System.out.println( name + " can eat");
    }
    protected void CanSleep(){
        System.out.println(name + " can sleep");
    }
    protected void CanReproduce(){
        System.out.println(name + " can reproduce");
    }

    protected abstract void MakeNoise();   // the added abstract method
}
```

Abstract method **doesn't need** a definition when declaring it because well it's **abstract** (*ba dum tss*). The definition  will be implemented on those concrete classess that derived from the animal class. Now, every class that derived from the animal class **MUST**  (keep this in mind) **implement** with ofcourse a defnition the MakeNoise() or else the compiler will show an error. Think of it as a contract. Here is the dog and cat implementation of MakeNoise():

``` java
public class Cat extends Animal {

    public Cat() {
        this.name = "Cat";
    }

    @Override
    protected void CanEat() {
        super.CanEat();
    }


	// Here is the implementation of the abstract
    @Override
    protected void MakeNoise() { 
			System.out.println("Meow Meow");
    }
}


public class Dog extends Animal {
    public Dog() {
        this.name = "Dog";
    }

	// Here is the implementation of the abstract
    @Override
    protected void MakeNoise() {
        System.out.println("Woof Woof");
    }
}


public class Abstraction {
    public static void main(String[] args) {

        Dog dog = new Dog();
        dog.CanEat();
        dog.CanSleep();
        dog.CanReproduce();
        dog.MakeNoise();  // BOOM! dog woofs


        Cat cat = new Cat();
        cat.CanEat();
        cat.CanSleep();
        cat.CanReproduce();
        cat.MakeNoise();    // BOOM! cat meows

    }
}

Output:
Dog can eat
Dog can sleep
Dog can reproduce
Woof Woof     -----> Dog woofs
Cat can eat
Cat can sleep
Cat can reproduce
Meow Meow    -----> Cat meows

```


### When do we use abstract method?
The CanEat(), CanSleep(), and CanReproduce() methods are all **concrete methods**(meaning all these methods have their definition on the same class they were declared). Why not make it abstract? Well think about it. Is there any animal at all that sleep differently? Like  sleeping with their eyes open perhaps? Ofcourse not. So there really is no point of having each class have their own implementation of these method if every animal sleep the same. This logic can be also applied to our other two method.  Then **when exactly** should we use abstract method?  Answer is, if you want to **require** your **certain class** to implement a certain method but with its own definition. That is in our example above, we required our cat and dog class to be able to make a noise via MakeNoise() but each with their own definition. Cause you know, every animal make different kind of sound.


### Pitfalls of using abstract method
Supposed one of your angels want to make a bird? Should we add a CanFly() method on our abstract class? Well let's see:


``` java
public abstract class Animal {

    public String name = "Animal";

    protected void CanEat(){
        System.out.println( name + " can eat");
    }
    protected void CanSleep(){
        System.out.println(name + " can sleep");
    }
    protected void CanReproduce(){
        System.out.println(name + " can reproduce");
    }

    protected abstract void MakeNoise();   // the added abstract method
    protected abstract void CanFly();    // Can fly?
}
```

Huh! looks okay. Let's check the implementation:

``` java
public class Dog extends Animal {
    public Dog() {
        this.name = "Dog";
    }

    @Override
    protected void MakeNoise() {
        System.out.println("Woof Woof");
    }


    // Dogs can fly? what?
    @Override
    protected void CanFly() {
        System.out.println("Uhm dogs can fly? Seriously?");
    }
}
public class Cat extends Animal {
    
    public Cat() {
        this.name = "Cat";
    }

    @Override
    protected void CanEat() {
        super.CanEat();
    }

    @Override
    protected void MakeNoise() {
        System.out.println("Meow Meow");
    }

    // Cats can fly? what?
    @Override
    protected void CanFly() {
        System.out.println("Uhm cats can fly? Seriously?");
    }
}

```

Woah Woah Woah! Cats and dogs can fly? That doesn't look right. Clearly something is wrong with our animal class. We shouldn't put the CanFly() there. So then where else should we put it? What if we create a new abstract class name CanFly with a Fly() method? Like this:
``` java
public abstract class CanFly {
    abstract void Fly();
}
```
Then derived from it? Nope! Too bad we can't! You see you can only **EXTEND** to **ONE** class or object in java. It was by design. So how are we going to fix this? Fortunately for us there is **interface** which we'll cover on the next topic. Hopefully though you gain a bit of understanding about java abstraction, when and how to use abstract classes and methods. Happy coding!