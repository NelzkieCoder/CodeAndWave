---
title: Java - Pass by value vs Pass by Reference
layout: post
image: https://postimg.org/image/vefxherup/
feature-img: assets/img/unsplash/coding.jpg
tags:
- Java
- OOP
keywords: Java, OOP, Pass-by-value, Pass-by-reference, Pass-by-pointers, Java Pointers,
  Java Reference
lang: en
category: articles
publisher: CodeAndWave
---

Never even heard of copy by value and copy by reference until I started learning a bit about c++. Back then I was just coding in java without even knowing if the variable I was passing is pass by value or reference......or pointer. Learning this will change the way you analyze things and also help you design your program in a better way. Now the question is, is java pass by value, or by reference ........ or by pointer?(*Hehe, just to add some twist here*).

 If you google it you will see a lot of articles about this.  My  goal is to help newbies to understand what is pass by value and what is pass by reference or pointer.

### First things first
 It should be clear that primitive types  such as
*     boolean
*     byte
*     short
*     int
*     long
* 		char
* 		double
* 		float

are all pass by value.

That means when create a variable `int a = 42` , the variable `a` is stored in the memory together with its value.

**But how about objects?** Well objects are not primitive, so that means they are pass by......... reference or pointers? *Hehe let's find out.*

### What is pass by value?

A pass by value means that when you pass a  primitive variable to a function, that variable is not pass but the **value**.

**Example**

``` java
static class PassValue{
        void passMePrimivite(int number){
            System.out.println(number);
        }
    }
    public static void main(String [] args)
    {

        PassValue passValue = new PassValue();
        int sample = 42;
        passValue.passMePrimivite(sample);
    }
```

Here, we are passing 42 on the `passMePrimitive` parameter. Keep in mind we are not passing the variable itself, instead we are **passing the value**. Thinking this way will save you a lot of headache.

### What is the difference of a reference and a pointer?


**What is a reference ?**
*  A reference is sort of like an alias for a variable. Think of yourself as having a nickname(pretty sure you have one). 
*  A reference shares the same memory space  with variable, after all its only an alias
*  A reference can't be null.
*  It can't also be reassigned.

**What is a pointer?**
* A pointer is a variable  that is pointing to an object created somewhere  in the memory.
* A pointer can be null.
* Can also be reassigned to a different variable
* It has its own space on the memory.

Example

`Person p = new Person();`

In here, you are you are declaring a **pointer** (p) with a type of Person **pointing to** the person object stored somewhere else in the memory. The key to understanding here is the keyword **new**. When you call `new Person()`, you are actually creating an object that will be **stored somewhere in** the memory. **Keep this in mind**.

[![pointer]({{site.baseurl}}/assets/img/blog/pointer.png)]({{site.baseurl}}/assets/img/blog/pointer.png)


### So is java pass by pointer then?
Well kind of. The official java spec says java is **Pass by value**. Why is it so?
Say for example
``` java

public static void main(String [] args)
    {
        Person p = new Person("zelda");   // address 25
        System.out.println(p.getName());
       changeValue(p) // here we are passing the value 25 which is the address.

        p = new Person("Link");   // address 10, p is now pointing to a new object somewhere in the heap with address of 10
        System.out.println(p.getName());
    }

    static  void changeValue(Person p){
        p.setName("john");
    }
```

Here,  we create a pointer that is poiting to a **Person** object, and then we pass that pointer to the changeValue() with parameter of type Person. What we are actually passing is the actual  address that our pointer p is pointing to(in our case it's 25), to our changeValue(). Then next we point that pointer **p ** to a new **Person** object which is located somewhere in the memory(in our example, the address would be 10).



### Conclusion

There is no such thing as a reference in java. Its only **pointers** and **values** but **when you are passing some objects, it is the address value that is being pass.**

Till next time, just code and wave.