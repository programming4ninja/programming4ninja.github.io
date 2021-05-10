---
title: "Exploring Java 8/4 : Method & Constructor Reference in Java 8"
date: 2021-04-01T00:06:36+05:30
description: "java 8 method reference operator; java 8 constructor reference operator; double colan operator in java 8; :: operator in java 8"
weight: 4
tags : ["Exploring Java 8"]
---

Hello there techie ninjas, I hope you are doing well. 
Welcome to Programming 4 Ninja and another blog on Method Reference & Constructor Reference in the Exploring Java 8 Series. 
Let's get started with the content then.


### What is the Method Reference in Java 8?
Method Reference is another way of writing concise lambda of one line, in case your lambda is just calling a 
function on an object, or a static function on class. 

*let's take the example of the below lambda*

```java
import java.util.function.Consumer;

public class Example {

    public void printStringInUpperCase(String str) {
        System.out.println(str.toUpperCase());
    }

    public static void main(String args[]) {
        Example example = new Example();
        Consumer<String> uppercaseConverterAndPrinter = 
            (str) -> example.printStringInUpperCase(str);
        uppercaseConverterAndPrinter.accept("hello world !");
    }
}
```


In the above code block, the line number 11,12 can be written more concisely like below,

```java
Consumer<String> uppercaseConverterAndPrinter = 
    example::printStringInUpperCase;
```
This type of declaration is called a method reference. Syntax of method reference is

```java
objeOfClass::methodWhichCanSatisfyInterfaceContract;
className::staticMethodWhichCanSatisfyInterface;
```

So In the above example, we have used the Consumer interface, And you might be aware from the learning of the previous Blog,  
Exploring Java 8: Functional Interface that the consumer interface accepts a parameter of type T and won't return anything. 
Here in the above example, it satisfies the contract, then function for which we have created a method reference "printStringInUpperCase", 
it accepts a parameter of type String and does not return anything.

*Some more example around the Method Reference & Static Method Reference*
```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class ConsumerMethodReferenceExample {

    public static void stringConsumerStaticMethod(String str) {
        System.out.println(str);
    }

    public void stringConsumerInstanceMethod(String str) {
        System.out.println(str);
    }

    public static void main(String[] args) {
        
        ConsumerMethodReferenceExample instance = new ConsumerMethodReferenceExample();
        Consumer<String> stringConsumerLambda = (str) -> ConsumerMethodReferenceExample.stringConsumerStaticMethod(str);
        Consumer<String> stringConsumerMethodReference = ConsumerMethodReferenceExample::stringConsumerStaticMethod;
        Consumer<String> stringConsumerMethodReferenceWithObject = instance::stringConsumerInstanceMethod;
        Consumer<String> stringConsumerMethodReferenceWithCleanerCode = System.out::println;

        stringConsumerLambda.accept("ABC");
        stringConsumerMethodReference.accept("ABC");
        stringConsumerMethodReferenceWithObject.accept("ABC");
        stringConsumerMethodReferenceWithCleanerCode.accept("ABC");

        List<String> listOfString = Arrays.asList("Apple", "Banana", "Cat", "Dog");
        System.out.println("Lambda Way");
        listOfString.forEach((str) -> ConsumerMethodReferenceExample.stringConsumerStaticMethod(str));

        System.out.println("Method Reference Way");
        listOfString.forEach(ConsumerMethodReferenceExample::stringConsumerStaticMethod);
        listOfString.forEach(instance::stringConsumerInstanceMethod);

        System.out.println("Method Reference Way more cleaner");
        listOfString.forEach(System.out::println);
    }
}
```

In the example we have seen method reference for static and non static function for consumer functional interfaces, 
Lets have a look at method reference for other type of functional interfaces.

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;
import java.util.stream.Collectors;

public class MethodReferenceWithFunctionalInterfaces {
    public static void main(String args[]) {
        MethodReferenceWithFunctionalInterfaces object = new MethodReferenceWithFunctionalInterfaces();

        Consumer<String> someConsumer = object::consumerKindOfMethod;
        someConsumer.accept("xyz");

        Supplier<Double> someSupplier = object::supplierKindOfMethod;
        System.out.println(someSupplier.get());

        Predicate<Integer> somePredicate = object::isEvenNumber;
        System.out.println(somePredicate.test(126) ? "Even" : "Odd");

        Function<List<String>, String> concatString = object::join;
        System.out.println(concatString.apply(Arrays.asList("I", "Am", "Java", "Ninja")));
    }

    // consumer sort of functional interfaces
    public void consumerKindOfMethod(String str) {
        System.out.println(str);
    }

    // supplier sort of functional interface
    public Double supplierKindOfMethod() {
        return Math.random();
    }

    // you can use predicate interfaces for this
    public boolean isEvenNumber(Integer number) {
        return number % 2 == 0;
    }

    //you cabn function functional interface for this
    public String join(List<String> list) {
        return list.stream().collect(Collectors.joining(" "));
    }
}
```

I hope you have learned the method reference in java 8. In short, it is just concise ways of implementation of functional interfaces. 
Lambda would be little longer way of writing it. but with method reference it is more concise, more readable and more short.

### Let's Start with Constructor Reference Now
Constructor reference is also similar to the method reference we have seen earlier.
There is one difference, constructor reference will always be of type Function Functional interface. 
it would accept a parameter to pass in the constructor and will always return the object of a class. 
Java has a Functional and BiFunction functional interface available in java.lang.function package. 
Function for single parameter input, BiFunction is for two input parameter for a constructor.

*Let's look at the below example which shows how you can code for Constructor reference*
```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;
import java.util.stream.Collectors;

class Things {
    private String name;

    public Things(String name) {
        this.name = name;
    }
}

public class ConstructorReferenceExample {
    public static void main(String args[]) {
        Function<String, Things> function = Things::new;
        Things apple = function.apply("Apple");

        /* Some real industry example about the constructor reference*/
        List<String> thingsString = Arrays.asList("Apple", "Ball", "Cat", "Dog", "Egg");

        List<Things> listOfThings = thingsString
            .stream()
            .map(Things::new) // accept a string and return object of class Things
            .collect(Collectors.toList());
    }
}
```

I hope you have understood the concept behind method reference and constructor reference introduced in java 8. 
The basic intention behind the method reference is to make your code concise, 
readable and allows you to pass a function as a parameter that can be invoked later point in time based on your logic.
it will allow you to build a code in a callback fashion, where you will declare your function, 
but it would be invoked far below in the call stack.


That's all for this blog ninjas. Let me know if you have any question, feedback, suggestion. 
I will always try to improve my blogs and will be happy to help you.

Till then Goodbye, Happy Coding !!!


[<< Chapter 3 : Functional Interfaces ](/exploringjava8/chapter3/) | [Chapter 5 : Introduction to Streams API >> ](/exploringjava8/chapter5/)
