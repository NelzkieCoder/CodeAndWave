---
title: Java 8 Lambda
layout: post
tagline: For android scheduling and notification
image: https://postimg.org/image/vefxherup/
feature-img: assets/img/unsplash/coding.jpg
tags:
- Lambda
- Java
keywords: Java 8, Lambda, Java Lambda, Android Lambda
lang: en
category: articles
publisher: CodeAndWave
---

Lambda expression is mainly used in functional programming such as Kotlin. I never plan to learn Java 8 Lambda before as I thought android won't support it. Everything change until I found a library for android that gives you the power to use Lambda. I must admit learning Lambda was quite hard.  It takes time to get used to syntax especially for someone who is used to object oriented programming. Hopefully this guide will help you understand the basics of Lambda in Java 8.

Before we start. Let's ask the question:

### Why do we need to learn lambda?
Well for one, it is mainly used in functional programming like kotlin. So, for anyone wants to learn kotlin or any other functional programming, learning lambda is a must. The other reason is, you could type less code but still had the same code logic as the other programmer who doesn't use lambda on his/her code. We programmers are naturally lazy (*admit it!*). It is the main reason why we automate things. This typing less code is one of the reason why I  like lambda.

### Lambda Syntax
Take a look at this code
```java
(a,b) ->a + b;
```

Now at first look, you might be thinking "*What on earth is that code?*". Well, I was like that when I first saw lambda expresion syntax. I know right. It felt like reading a hieroglyphics writing. Anyway,  what this piece of code is doing right here is just adding the value of `a` and `b`.


### The Basics
Suppose we have a Dog class that can bark. On Java 7 way, we do it like this:

```java
void bark(IBark bark){
        bark.woof();
    }
    public static void main(String[] args) {

        IBark bark1 = new Bark();
        IBark bark2 = new IBark() {
            @Override
            public void woof() {
                System.out.println("Woof Woof");
            }
        };

        Dog dog = new Dog();
        dog.bark(bark1);
        dog.bark(bark2);
    }

public class Bark implements IBark {
    @Override
    public void woof() {
        System.out.println("Wooof Wooof");
    }
}

public interface IBark {
    void woof();
}
```

With just that simple task we have to type a lot. For small code base that is not a problem but think about large code base with hundreds of lines of code and many classes. Now what if we don't want to create a **new class to make the implementation of the interface** or we don't want to make an **anonymous inner class** like the one we did to define **bark2**? What if we just want this single piece of code and pass it around like a value in a variable? Fortunately, we can on Java 8.

```java
 lambdaBark = () -> System.out.println("Woof woof Lambda");
```

One thing you will notice is that what is the **type** of `lambdaBark`? We'll see later on what its type would be but for now let's focus on the right hand side of the `=` sign.

This code  
```java
() -> System.out.println("Woof woof Lambda")
``` 
that we just pass on our variable `lambdaBark` is what we called **Function Literal**.

### What is Function Literal?

Take a look at this code

```java
String name = "CodeAndWave";
``` 
this `"CodeAndWave"` is what we called [String Literal](https://msdn.microsoft.com/en-us/library/8kc54dd5.aspx) this is the same as the function literal.  The 
 `() -> System.out.println("Woof woof Lambda")` can be **viewed** as string literal only that it is a function hence it is called Function Literal. **Think of it as assigning a block of code to a variable or passing a block of code to a variable or to a method**. Hope that makes sense.
 
###  Lambda syntax
Imagine if we could pass the function  to a variable like this:

```java
HelloLamdba = public void Hello(){
            System.out.println("Hello there");
        }
```

Obviously if you try this on IDE it will show an error. In order to make it acceptable for our IDE, we first must remove and replace some part of the code. First, do we need the **public** modifier? On this case no, **we don't need the modifier**. Second, do we need the `void` keyword? Yes! But in lambda a **void function** is express as `()` - an open and close parenthesis with nothing inside of it.  Next, do we need the name `Hello`? In lambda, no we don't need to define a name of a function so we remove that. Now, in order to denote to our java compiler that we are declaring a lambda expression we have to use the **arrow operator** `->`.  The code would now look like this

```java
 HelloLamdba =  () -> {
            System.out.println("Hello there");
};
```

This is a perfectly valid code but to make it more simpler. **If we are passing a single line function literal in lambda we could omit the curly braces {}** so it will be like this
```java
HelloLamdba =  () -> System.out.println("Hello there");
```


###  What about the variable data type?
In this code 

```java
HelloLamdba =  () -> System.out.println("Hello there");
```
How do we know what is the type of HelloLambda? Well unfortunately java engineers didn't add any new data type in java but instead make use of the power of **[interface]({% post_url 2017-11-25-java-interface %})**. Abstract method declared on our interface **must match the signature of our lambda expression** in order to make our interface the data type of our `HelloLambda` variable.

```java
interface HelloGreeter{
        void sayIt();
    }
```

Here our `sayIt()` is a void and doesn't return anything. It's the same as our lambda expression it is a void and doesn't return anything. You can only use a **function interface**as a data type when declaring a lambda.

### What is Function Interface
A Function Interface is an interface with only one abstract method. To restrict other people from adding another method on our functional interface we use the `@FunctionalInterface` annotation in Java 8.

### Lambda Syntax with arguments

Say we want to have a function that would add two numbers

On Java 7 we would do it like this

```java
public class CalculateArgs {

    interface Calculate{
        int add(int a, int b);
    }
    void addTwoArgs(int a, int b,Calculate calculate){
        System.out.println(calculate.add(a,b));
    }

    public static void main(String[] args) {

        CalculateArgs lesson2 = new CalculateArgs();

        Calculate calculate = new Calculate() {
            @Override
            public int add(int a, int b) {
                return a + b;
            }
        };
        lesson2.addTwoArgs(5,5,calculate);
    }
}
```

But on Java 8 lambda it's pretty easy.

```java
    public static void main(String[] args) {

        CalculateArgs lesson2 = new CalculateArgs();

       Calculate c = (int a, int b) -> a + b;
        lesson2.addTwoArgs(5, 5, c);
    }
```

A bit confusing but here take a look

[![]({{site.baseurl}}/assets/img/blog/lambda2.png)]({{site.baseurl}}/assets/img/blog/lambda.png)

The value 5 with <span style="color:red" >red</span> underline is pass to variable `a` and the other 5 with <span style="color:green" >green</span> underline is pass to variable `b` and notice that the function literal `(a, b) -> a + b` is exactly the same as on the anonymous inner class `add()` we define. The only difference is that on java 7 we have 3 line of code while on java 8 lambda its only one liner. Also, if lambda expresion is one liner, we can just **omit** the **return** keyword if it is returning something.

#### Type Inference
We could omit the data type `int` of `a` and `b` to make it look like this
```java
Calculate c = ( a,  b) -> a + b;
```

But how does java would know its data type if remove it? Well, you see our function interface?

```java
 interface Calculate{
        int add(int a, int b);
    }
```
The abstract method `add` has an argument with a data type **explicitly** defined. That means that since we are using this function interface on our lambda expression, the **compiler is smart enough** to  **infer or deduce** the data type for the argument on our lamdba expression base on our function interface. Pretty cool. It can also infer its return type. Take a look at this(sorry about my drawing. I'm really bad at it)

[![]({{site.baseurl}}/assets/img/blog/function_interface.png)]({{site.baseurl}}/assets/img/blog/function_interface.png)



### Multiple line lambda expression
We've been practicing on one liner code in lambda. But what if we have multiple lines of code? Pretty easy. We only have to add curly braces. and if it is returning something, add the **return** keyword That's about it.

```java
Calculate cc = (a,b)->{
          if(a == 0){
              a = 1;
          }
          return a + b;
        };
```

### Method Reference

Take a look at this code.

```java
public class HelloThere {

    public void Hello(HelloGreeter   helloGreeter){
       helloGreeter.sayIt();
    }
    public void HelloMethodRef(){
        System.out.println("Hello there from MethodRef");
    }

    interface HelloGreeter{
        void sayIt();
    }
    public static void main(String[] args) {

        HelloThere helloThere = new HelloThere();
        helloThere.Hello(() -> helloThere.HelloMethodRef());

    }
}
```

This   
``` java
helloThere.Hello(() -> helloThere.HelloMethodRef());
``` 
is no different from the one we were doing before. We just pass the method itself and not the expression. That is valid. Now in Java 8 there is a feature called **Method reference**. Basically **if the argument is the same as the method parameter,** we can convert it to method reference. To convert to method reference we must first make sure that the argument in our code is void and the method we are passing doesn't have a parameter. `HelloMethodRef` doesn't have any parameter and that our lambda is void and not returning anything we can make it like this

```java 
helloThere.Hello(helloThere::HelloMethodRef);
```

#### Another example with arguments
For a code with arguments it the same as without one

```java
 interface Calculate {
        int add(int a, int b);
    }

    void addTwoArgs(int a, int b, Calculate calculate) {
        System.out.println(calculate.add(a, b));
    }

    public static int addition(int a, int b){
        return a + b;
    }
    public static void main(String[] args) {

        CalculateArgs lesson2 = new CalculateArgs();

        lesson2.addTwoArgs(5,5,CalculateArgs::addition);

    }
```

I wouldn't worry too much about method reference. If you are using Intellij Idea, the IDE will suggest to use method reference. Depending on your key biding. Mine is alt-enter, the IDE would replace the lamdba expression with method reference if you want to. I'm not sure about Eclise IDE though.

### Conclusion

Learning lambda is pretty hard especially for someone who is used to OOP. It requires a different thinking pattern. But with practice and patience you will soon understand and like this awesome feature of Java 8. The next topic I will talk about how to implement Lambda on android. It will be pretty easy now that we have covered the basics of lambda. Until code and wave peoepl just code and wave.