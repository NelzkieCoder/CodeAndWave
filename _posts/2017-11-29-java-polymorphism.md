---
title: Java - Polymorphism
lang: en
category: articles
layout: post
tagline: Third pillar of OOP
image: "/assets/img/profile-small.png"
header:
  image: "/assets/img/profile-small.png"
tags:
- Java
- OOP
keywords: Java, OOP, Polymorphism, 
date: '2017-11-29 08:13:28 +0000'

---

Polymorphism is the ability of an object to **transform to different forms**. "*Poly*" means "many" and "*morphs*" means "forms" so meaning, "*many forms*". Polymorphism can only be achieved through [inheritance]({% post_url 2017-11-11-Java-Inheritance %}), meaning by extending to a base class.

<!--break-->

## Polymorphism through [inheritance]({% post_url 2017-11-11-Java-Inheritance %})

``` java
Animal b = new Bird(); // polymorphism in action
```
			
Here, since the base class of bird is animal, we can declare the data type as Animal and define it as Bird. If you think about it " *a bird is an animal*" which is **true**. What we are saying here is " *I am transforming this animal to a bird* " sort of but you get the idea.  In polymorphism, a derived class can **modify** the default traits inherited from its base class and also have its **own unique traits**. Like in our example, we have the **fly()** in our Bird because we implemented an interface called HasWings(the original name of the interface is CanFly but I changed it to HasWings cause it makes more sense).  The downside of this approached is you can't call a method that is unique only from derived class but only those method that is declared on the base class. Meaning you can't call a Fly() of the bird class like this.

``` java
 Animal bb = new Bird();
 bb.CanSleep();
 bb.CanReproduce();
 bb.CanEat();

 bb.fly(); // No, cant be called, animal class doesn't have this method
```

Why can't we call the fly()? Well that's because our **data type** is the Animal object which  **doesn't** have that method. If you want to call the fly() you have to declare the bird  like this: `Bird b = new Bird();`  then you could call the fly().

We also can't do this:

``` java
Bird bird = new Animal();  // This is wrong
```

What we are saying here is "*An animal is a bird* ", but you have to remember that not every animal is a bird. So this statement is false. *Every bird is an animal but not every animal is a bird*.


## Polymorphism through [interface]({% post_url 2017-11-25-java-interface %}).

Well not really, but we can  achieve a "**polymorphic**" behavior using interfaces. How? 

Well since our Bird class implements a HasWings interface which has a fly(), we can do this:

``` java
   HasWings b = new Bird(); // polymorphic behavior 
```

Its like saying " *a bird has wings* " which is ofcourse true. So in a sense, this isn't actually polymorphism but rather having a **polymorphic behavior**. Big difference there how? Well check this:

``` java
HasWings  p = new Pegasus();
p.fly();
```

Obviously a pegasus is **not really** a bird but a  mythical creature. So what we are  saying is " *a pegasus is not a bird, but rather, it has wings that it can use to fly* ". The behavior that the pegasus has is that it can fly. We are not doing any transformation here,  cause you can't say " *I am transforming this HasWings to a pegasus* " yeah, that doesn't make any sense at all. That's why this isn't really polymorphism.


## Usecase
Just like in other example, supposed you are a god and want every animal to go to sleep with just one command. How can you do it? What if, we create a command that will call every animal's sleep method? Let's see:

``` java
public static void main(String[] args) {
        Bird b = new Bird();
        Cat c = new Cat();
        Dog d = new Dog();
        GodSaidGoToSleep(b,c,d);
    }


    public static void GodSaidGoToSleep(Animal... animals){
        for (Animal animal: animals) {
            animal.sleep();   // baam! they're now going to sleep
        }
    }
```

See what we did here? Since every one of them inherits from our animal class, we could just pass the **b**,**c**, and **d** to the **GodSaidGoToSleep()** with a parameter of animal object, instead of making a duplicate command with different signature for each of them.  ***Short, simple, and elegant***.


### Another example of polymorphic behavior

What if you want to call all the things that has wings and make them all fly? You don't care if it's a bird or a mythical creature.  How can you do it? Easy


``` java
public static void main(String[] args) {

        Pegasus p = new Pegasus();
        GodSaidToFly(b,p);
    }

    public static void GodSaidToFly(HasWings... objects){
        for (HasWings objectThatHasWings: objects) {
            objectThatHasWings.fly(); // Baam! they're now flying on your command
        }

    }
```





Polymorphism is a bit confusing for a beginner and is  a hard topic to explain with just a code only. Even I was confused about this before. Keep in mind that this is my own interpretation of polymorphism but I do hope that with my example and explanation, you kinda gain a bit of understanding about how polymorphism works. Till next time, just code and wave coders, just code and wave.