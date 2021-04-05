---
title: "Chapter 9 : Optionals in Java"
description: "Java 8 Optional class example; java how to avoid null pointer exception"
date: 2021-04-03T23:55:13+05:30
draft: true
weight: 9
tags : ["Exploring Java 8"]
---

Hello there techie ninjas, I hope you are doing well. Welcome to programming 4 ninja blogs. 
Today in this blog we would be learning about the new class java.util.Optional which was introduced as part of java8.
Let's get Started then.

### What is Optional ?
Optional is class introduced as part of java 8 release, which act as a wrapper around your object. Optional are most 
widely used to avoid null values. It tells you whether the object is empty(null), or it has value.
 

### Why one should use Optional ?
Java introduce Optional in order to avoid the usage of null values. Many advanced programming languages like Kotlin,
avoid usage of nullable value. They restrict usage of null values. Of course there is way of using nulls in Kotlin, but
it is not at all recommended to use nullable values. Null values introduce too many problems than it actually solves.
Like, whenever you are using any DB interacting apis you will never be sure about whether api will give you object or 
null value. Uncertainty would be always there. Many would end up assuming that it would never be an empty (null) object 
that flow someday blast with NullPointerException.

Optional is the safest way of writing program and avoid null. It's not like that without Optional, you cannot write null 
safe code, but you will end up writing so much common boilerplate code. Optional has rich set of API, which has written 
those common code, like 

* check if object is empty
* return object if it is not empty
* return default object if it is null
* throw exception if object is null
* convert object to another if it is not null

Now since we know little behind the sense for the Optionals, lets see how to use

### How to use Optional ?

Let's have a look at below example,

```java
package org.java4ninja.exploringjava8.optional;

import java.util.Optional;
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.ToString;

public class OptionalDemo {

    @AllArgsConstructor
    @ToString
    @Getter
    static class Employee {
        private int id;
        private String name;
    }

    public static void main(String args[]) {

        Optional<Employee> employeeOptional = findEmployeeById(1);

        // check if the object present
        boolean present = employeeOptional.isPresent();

        // get the object out of wrapper
        Employee employee = employeeOptional.get();

        // transform Optional object, only if it is not empty
        Optional<String> nameOptional = employeeOptional.map(Employee::getName);

        // filter Optional object only if it is not empty
        Optional<Employee> employeeOnlyIfIdIsOne = employeeOptional.filter(e -> e.getId() == 1);

        // consume object only if it is not empty, in this case our consumer is printing it on console
        employeeOptional.ifPresent(System.out::println);

        // get object or else get a default object
        Employee employeeIfNotExistGiveDefault = employeeOptional.orElse(new Employee(0, "Not Available"));


        Optional<Employee> notAvailableEmployee = findEmployeeById(1);

        // get object or else get a default object by supplier
        Employee employee1 = notAvailableEmployee.orElseGet(() -> findEmployeeById(0).get());

        // get object or else throw exception
        notAvailableEmployee.orElseThrow(() -> new RuntimeException("Employee Not Available"));

    }

    public static Optional<Employee> findEmployeeById(int id) {
        if (id == 1) {
            return Optional.of(new Employee(1, "Anil"));
        }
        return Optional.empty();
    }
}
```


[<< Chapter 1 : ](/exploringjava8/chapter1/) | [Chapter 3 :  >> ](/exploringjava8/chapter2/)
