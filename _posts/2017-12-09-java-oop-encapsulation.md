---
title: Java OOP - Encapsulation
layout: post
tagline: The Fourth Pillar of OOP
image: "/assets/patterns/paisley.png"
feature-img: "assets/img/unsplash/coding.jpg"
tags:
- Java
- OOP
keywords: Java, OOP, Encapsulation
lang: en
date: '2017-11-11 08:13:28 +0000'
category: articles
---

## What is encapsulation?
Encapsulation is a way of **hiding the implementation** to the other object. It is also know as "***data hiding***".  It means making all your fields private and will only be accessible by **field accessors**(in java term) in c-sharp term, its **properties**.

## Why use encapsulation
Well IMO it is to prevent other objects from accidentally changing the data of that field specific for that class therefore preventing unnecessary bug.

**Example**

``` java
public class Employee {

    public String firstname = "Code";
    public String middlname = "And";
    public String lastname = "Wave";
    public int age = 18;

    @Override
    public String toString() {
        return  " Hi, Im " + firstname + " " + middlname + " " + lastname + " age " + age  ;
    }
}

 public static void main(String[] args) {
        Employee employee = new Employee();
        employee.lastname = "booooo";   // ooops
        System.out.println(employee);

    }
```

Now this won't make sense on a small code. But imagine a large code base and out of stress you accidentally change a data that isn't supposed to be change. BAAAM! Your doomed. You gotta find what class or object is the one changing the data.  And we all know bug hunting is not fun at all. We all hate it.


## How to use encapsulation

To use encapsulation we take advantage of accessors or properties.

``` java
public class Employee {

     private String firstname = "Code";
    private String middlname = "And";
    private String lastname = "Wave";
    private int age = 18;

    public String getFirstname() {
        return firstname;
    }

    public void setFirstname(String firstname) {
        this.firstname = firstname;
    }

    public String getMiddlname() {
        return middlname;
    }

    public void setMiddlname(String middlname) {
        this.middlname = middlname;
    }

    public String getLastname() {
        return lastname;
    }

    public void setLastname(String lastname) {
        this.lastname = lastname;
    }

    public int getAge() {
        return age;
    }

public static void main(String[] args) {
        Employee employee = new Employee();
        employee.setLastname("Cool and Awesome "); // Deliberate modification of data
        System.out.println(employee);
    }
}
```

Like I said this won't probably make sense on small  code base, but if you are working on **large code**, you wouldn't wanna accidentally change a field's data critical on your app just because you are too lazy to use accessors and setting the field to private. **Remeber**, by default when you declare a field on java, it's default access modifier is **protected** so we have to manully set it to **private**.


## Conclusion
No matter the size of our code base, it is a **good practice to always encapsulate our code**. Yeah, I know we are lazy and we don't want to type too much so it can be tempting to just make all fields public, but with IDE becoming  smarter than it was, it  only take two keystroke on Intellij to make a field accessors(***getter and setters***),  so there really is no reason not to encapsulate our code unless...  unless  you want headache.