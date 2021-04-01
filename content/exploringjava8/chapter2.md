---
title: "Chapter 2 : Lambda In Java"
date: 2021-03-31T00:53:47+05:30
weight: 2
description: "Lambda function tutorial in java 8 with example"
tags : ["Exploring Java 8"]
---

Hello there techie ninjas, Hope you are doing well. Today in this blog we would be learning about Lambda, 
Functional interfaces, and we are going to see a lot of examples about Lambda which can tell us with the invention of Lambda how much code has become readable. 
Lambda is the coolest feature of Java 8.


### What are so cool things about Lambda?
* Till now, you have seen passing objects, primitives to the method as a parameter, with lambda coming in the picture, you can pass a function as a parameters
* functions are treated as the first-class citizen now
* Your code will become so easy to read, and a lot of boilerplate would get removed
* Lambda is nothing, but a shorter way of implementing the functional interface
* Java 8 bundled with a bunch of handful functional interfaces which is very helpful for building blocks


*Let's start exploring Lambda, But before that have look at below classical example*
```java
class MyThread implements Runnable {
    @Override 
    public void run() {
        System.out.println("Hello From Thread !");
    }
}

public class LambdaExampleOldWay {
    public static void main(String args[]) {
        Thread t1 = new Thread(new MyThread());
        t1.start();
    }
}
```

In the above example, there is a lot of boilerplate code exists. 
if we are not maintaining a state, we shouldn't be needed to create a class just to declare a function. 
Now let's have a look at a new way of writing the same code in Lambda.

*Solving a problem using Lambda way*
```java
public class LambdaExampleOldWay {
    public static void main(String args[]) {
        Thread t1 = new Thread(() -> {
            System.out.println("Hello from Lambda Thread !!");
        });
        t1.start();
    }
}
```



Simple, Clean, and Straight forward. 
Easy to understand. 
No Need for declaring class. 

you must have understood the syntax for the declaring lambda. Here are the syntax and points about lambda

```java
    (parameter1, parameter2, . . . .) -> {
        body of lambda function
    }
```

* We declare lambda by arrow function -> symbol
* Before arrow function, we have to specify parameters to the lambda functions
* Followed by opening and closing curly braces with body specified within it
* Specifying the data type for parameters is optional
* Specifying the curly braces and return statement is optional if your lambda is a one-liner statement


###  Let's have another example of Lambda for sorting elements in the list
```java
public class LambdaExample {
    public static void main(String args[]) {
        List<Integer> listOfNumbers = Arrays.asList(10, 15, 30, 35, 22, 12, 9);
        listOfNumbers.sort((num1, num2) -> num1.compareTo(num2));
        System.out.println(listOfNumbers);
    }
}
```

* In the above example,  sort accepts an object of the Comparator<T> interface.
* We have implemented the compareTo method of Comparator<T> using lambda.
* So behind the scene, Lambda is nothing but an anonymous implementation of the function in the interface.


### Parameter and return type identification in Lambda function
Now, the question is how will the lambda come to know the type of parameter, 
since parameter type declaration is optional and return type are not needed?

Basically, lambda is nothing but the anonymous method implementation of the interface. 
In this case of the sort, the method expects that the passed object should be implementing 
the Comparator<? super Integer> interface.Our lambda is implementing the same thing. 
Our lambda is satisfying the only method in the comparator.

```java
@java.lang.FunctionalInterface
public interface Comparator <T> {
    int compare(T t, T t1);
}
```


if you compare our lambda format, the number of parameters it accepts, 
the return type of the function is matching with our lambda.
Our lambda is creating anonymous objects behind the scene only.


Now the One more question arises. The interface can have multiple abstract methods. 
Then which one our lambda will pick for matching its arguments?

The answer is that We can create a lambda function only for those interfaces which have only one abstract, unimplemented method in them. 
Java 8 Call them a **Functional Interface**. An interface that supports the Functional Style of programming. 
We would be going to learn more about the functional interface in the next lesson.

Till then stay tuned, and See you all in the Next Lesson

[<< Chapter 1 : What's New in Java 8](/exploringjava8/chapter1/) | [Chapter 3 : Functional Interfaces >> ](/exploringjava8/chapter3/)
