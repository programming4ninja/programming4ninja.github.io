---
title: "Chapter 3 : Functional Interface"
date: 2021-04-01T00:06:33+05:30
weight: 3
tags : ["Exploring Java 8"]
---

Hello there techie ninjas, I hope you are doing well. 

Today in this blog we would be exploring Functional interfaces that were introduced in Java 8. 
Although we talked about functional interfaces a little bit, this blog would be completely hands-on on functional interfaces. 
We would be covering the Why, What, and How for the functional interfaces. 
If you haven't check out my previous blog "Lambda in java" please do checkout.
That would set the stage for this blog. Let's get started then.

### What is Functional Interface?
As we have seen in the previous blog, Functional Interface in Java 8 is an Interface in java with only a single abstract method. 
All Functional Interfaces are annotated with @FunctionalInterface annotation.


### Why there is a need for a Functional Interface, or An Interface with only one abstract method?

With Java 8, Java is set the stage for supporting the functional programming style paradigm. 
Functional Programming says that you should treat the function as a first-class citizen, Object-oriented programming emphasis mutating the state of objects, while Functional programming says that you should never mutate the state of the object, 
instead your function should create immutable objects; and name of the function should clearly specify the final state of objects. 
For each such function, we can have a functional interface. 
Such a functional interface can be implemented very well using Lambda, and it would be easily readable and clean code.
Although you would rarely need to create one since java 8 has a whole bunch of functional interfaces for n number of operations. 
You might see this information not so clear, but when you start programming in a functional style, you will come to know the advantage of it.



### How to Declare your own functional interface?

It would be very easy, you can create a normal interface in java. 
To make your interface a functional interface, make sure it has only a single abstract un-implemented method. 
That all it required. 
Annotating your interface with @FunctionalInterface will tell the compiler that this interface would be used as a functional one, don't allow anyone in the future to add more methods. 
if so give compile error about that. 
However, it is optional, but a good safety net to work in a collaborative team.


*Let's have look at below basic example of declaring a functional interface*
```java
@FunctionalInterface
interface MyInterface {
    public void doSomething();
}
```


### Basic Functional Interfaces available in Java 8
Java 8 comes with a bunch of functional interfaces which are getting used in many of the Java 8 features like streams. 
There are some common functional interfaces that you will definitely face while doing functional programming.
We would be exploring those. Once you are familiar with those, I am pretty sure that you will learn which one to use in which scenario. 
So let's get started with it.


##### *Consumer Functional Interface*

As the name tell this functional interface act as a consumer. 
Let's first understand what do you mean by a consumer. 
A consumer is something, which accepts input and processes it.  
It won't give feedback to the caller. That means, it did not return anything.
So while programming, if you come across such a scenario, you can use the consumer interface.
A typical example would be printing something, write something to the database, send something to another client.

This is how the functional Interface look like for the Consumer

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

Below is an example of a consumer example that consumes string and prints it on the console.

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class ConsumerExample {
    public static void main(String args[]) {
        List<String> listOfStrings = Arrays.asList("Apple", "Ball", "Cat", "Dog", "Egg", "Fish");

        /* Easy way for beginners */
        Consumer<String> printConsumer = (input) -> System.out.println(input);
        listOfStrings.forEach(printConsumer);

        /* Cleaner and shorter way */
        listOfStrings.forEach((input) -> System.out.println(input));
    }
}
```



#### *Predicate Functional Interface*

As the name suggests, this interface has a method, named test. 
it will act as a predicate for the input. You can pass input to it. 
function body will have a code that can determine whether the pass input is valid for some condition, and it will return boolean. 
Let's have a look at the predicate functional interface available in java 8.

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

let's have a look at the below example, which demonstrates the usage of the predicate interface. In the below example, we have used the Streams API, which was introduced in java8. You might not be familiar with that, but as of now, you can think it is like an iterator on the lists. We would be going to learn about Streams API in-depth going ahead in this blog.

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class PredicateExample {
    public static void main(String args[]) {
        List<String> listOfStrings = Arrays.asList("Apple", "Ball", "Cat",
            "Dog", "Egg", "Fish", "Ground",
            "Horse", "Ice-Cream", "Juice",
            "Kite", "Lemon", "Mango", "Nest",
            "Orange", "Pears", "Queen", "Rat",
            "Sun", "Tv", "Umbrella", "Volvo", "Watch", "Xray", "Zebra");

        // Predicate which accept string, check its length and if it is 3 or less
        // then return true otherwise it will return false
        Predicate<String> predicate = (input) -> input.length() <= 3;

        List<String> listWithOnly4letter = listOfStrings
            // consider this as elements are iterating one by one
            .stream()
            // Applying our predicate, filter function which only
            // send those value to next level for which predicate return true
            .filter(predicate) 
            // collect all element who pass ed filter test
            .collect(Collectors.toList());

        System.out.println(listWithOnly4letter);
    }
}
```



#### *Supplier Functional Interface*

As the name suggests, the Supplier functional interface deals only with supplying values without any input. 
this is how the supplier interface looks like.

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```
Let's have a look at the below example, where we have created a double supplier of random values. one can simply invoke the get method of it and they will get a random value.

```java
import java.util.function.Supplier;

public class SupplierExample {
    public static void main(String args[]) {
        Supplier<Double> doubleSupplier = () -> Math.random();
        System.out.println(doubleSupplier.get());
    }
}
```


#### *Function Functional Interface*

A function interface acts as a converter sort of thing. 
it accepts one parameter of type X and can return the value of type X or Y.
whenever you have a use case of converting or transforming something into another form. 
you can use this interface.  below is the signature of the functional interface

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```
Let's have a look at the below example,  Again this example would be related to streams.
think this example similar to the predicate example.

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;
import java.util.stream.Collectors;

public class FunctionalInterfaceExample {
public static void main(String args[]) {

        List<String> listOfStrings = Arrays.asList("one", "two", "three");

        Function<String, String> transformer = (str) -> str.toUpperCase();

        String normalString = "abc";
        String upperCaseString = transformer.apply(normalString);
        System.out.println(upperCaseString);


        List<String> upperCaseStringList = listOfStrings
            .stream()
            .map(transformer)
            .collect(Collectors.toList());

        System.out.println(upperCaseStringList);
    }
}
```


There are lots of other functional interfaces available in java 8. 
explore other functional Interfaces added as part of Java 8 in the package "java.util.function"




That's all for this tutorial friends. stay tune on Java4Ninja. See you in the next Blog.

[<< Chapter 2 : Lambda In Java](/exploringjava8/chapter2/) | [Chapter 4 : Java Method Reference and Constructor Reference >> ](/exploringjava8/chapter4/)
