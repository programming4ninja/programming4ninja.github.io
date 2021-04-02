---
title: "Chapter 8 : Streams terminal functions"
description: "java 8 stream api terminal operation with example; java 8 streaming api terminal function allMatch, anyMatch, noneMatch, count, forEach, min, max, reduce, collect"
date: 2021-04-02T17:54:51+05:30
weight: 8
tags : ["Exploring Java 8"]
---

Hello there techie Ninja's, Hope you are doing well. Today, in this blog we would be learning about the 
terminal operations in java 8 streams. You might have seen in previous blogs that intermediate operation 
flows the data, transform it but it again produces new streams. If you want to collect the data from streams 
to normal collection class or another format, you need to use the terminal operation.  One of the terminal 
functions we have seen till now is Collectors.collect(). Yes, it is a terminal function. Apart from this one,
java streams have a few more terminal functions, which you can use as per need. Let's check this out.

## Terminal Functions/Operations in Java 8 Streaming API
* allMatch
* anyMatch
* noneMatch
* count
* forEach
* min
* max
* reduce
* collect


### allMatch(), anyMatch(), noneMatch()
These 3 termination functions accept a predicate and return a boolean value. 
These functions mainly used to evaluate a condition on the elements of a stream and return true or 
false based on the condition. The point to note here is that it is a termination function.
once you invoke it on stream, the stream will no longer be opened. it would be terminated once you call the
termination function. let's have a look at the below example

Signature :
```java
boolean anyMatch(Predicate<? super T> predicate);
boolean allMatch(Predicate<? super T> predicate);
boolean noneMatch(Predicate<? super T> predicate);
```
Example :
```java
package org.java4ninja.exploringjava8.streams;

import java.util.Arrays;

public class TerminalOperations {
    public static void main(String args[]) {
        // return true if all words have minimum 2 or more characters
        // expected to match the condition by all elements of streams
        boolean doesAllWordsHas3Letters = Arrays.asList(
            "ANT",
            "BAT",
            "CAT",
            "DOG",
            "EAT",
            "FAT").stream().allMatch((s) -> s.length() >= 2);

        // return true if at least one cat exist
        // expected to match the condition by at least by one element
        boolean isCatPresentInStream = Arrays.asList(
            "ANT",
            "BAT",
            "CAT",
            "DOG",
            "EAT",
            "FAT").stream().anyMatch((s) -> s.equalsIgnoreCase("cat"));

        // return true if none of the element matches the condition
        // expected to not to match
        boolean noneOfTheElementMatchCondition = Arrays.asList(
            "ANT",
            "BAT",
            "CAT",
            "DOG",
            "EAT",
            "FAT").stream().noneMatch((s) -> s.length() > 3);
        System.out.println(noneOfTheElementMatchCondition);
    }
}
```

### min() and max()
This terminal operation returns the smallest and biggest element in the stream. min() function return the smallest element whereas max() 
function return the biggest element. Both of them take the comparator as an input and return the Optional of the type. When Stream is empty this 
function will return Optional empty().

Signature:
```java
Optional<T> min(Comparator<? super T> comparator);
Optional<T> max(Comparator<? super T> comparator);
```

Example :
```java
package org.java4ninja.exploringjava8.streams;

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.ToString;

public class TerminalOperationMinMax {

    @AllArgsConstructor
    @ToString
    @Getter
    private static class Employee {
        private int id;
        private String name;
        private int age;
    }

    public static void main(String args[]) {

        List<Employee> listOfEmployees = Arrays.asList(
            new Employee(1, "Anil", 29),
            new Employee(2, "Joe", 21),
            new Employee(3, "Jane", 35),
            new Employee(4, "Bob", 26)
        );

        Employee youngestEmployee = listOfEmployees
            .stream()
            .min(Comparator.comparing(Employee::getAge))
            .get();

        System.out.println("Youngest Employee : " + youngestEmployee);

        Employee eldestEmployee = listOfEmployees
            .stream()
            .max(Comparator.comparing(Employee::getAge))
            .get();

        System.out.println("Eldest Employee : " + eldestEmployee);
    }
}
```

### count(),  forEach() and reduce()
count terminal function counts the number of elements in the stream and returns the long value.

forEach terminal function iterate over the element of the streams. forEach is a terminal function don't expect you will get the stream back from forEach function.

reduce is a reducer function that returns a single value representing the collection. if you have seen the map-reduce program, where map operation transforms the data and reduces operation reduce it to a single figure. reduce() function is the reducer part of a map-reduce program in the stream.

Signature
```java
long count();
void forEach(Consumer<? super T> action);
T reduce(T identity, BinaryOperator<T> accumulator);
Optional<T> reduce(BinaryOperator<T> accumulator);
<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);
```

Example
```java
package org.java4ninja.exploringjava8.streams;

import java.util.Arrays;
import java.util.List;
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.ToString;

public class TerminalOperationCountForEachReduce {

    @AllArgsConstructor
    @ToString
    @Getter
    private static class Employee {
        private int id;
        private String name;
        private int age;
    }

    public static void main(String args[]) {

        List<Employee> listOfEmployees = Arrays.asList(
            new Employee(1, "Anil", 29),
            new Employee(2, "Joe", 21),
            new Employee(3, "Jane", 35),
            new Employee(4, "Bob", 26)
        );

        long count = listOfEmployees.stream().count();
        System.out.println("There are " + count + " employees in office");

        System.out.println("========= Employee Detaills =======");
        listOfEmployees
            .stream()
            .forEach(eachEmployee -> {
                System.out.println(eachEmployee.toString());
            });

        Integer combinedAgeOfCompany = listOfEmployees
            .stream()
            .map(Employee::getAge)
            .reduce((firstAge, secondAge) -> firstAge + secondAge)
            .get();

        System.out.println("Combined Age of Company is " + combinedAgeOfCompany);
    }
}
```

### collect()
It is the most widely used terminal operation on stream. It takes Collector as an input and returns collection in various formats.
We would be going to see the collection with most of the use cases in the next another series with the name ["Java 8 Streaming API Examples"](/java8streamingapiexample). So we keep this topic for the new blog series


I hope you have enjoyed this tutorial. Please let me know your comments, suggestion, and improvements I will try to improve my blog.

Till then, Good Bye Happy Coding !!!

[<< Chapter 7 : Internal working of Streams](/exploringjava8/chapter7/) 
