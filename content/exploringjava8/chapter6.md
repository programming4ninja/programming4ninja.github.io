---
title: "Chapter 6 : Intermediate Streams Operations"
description: "Java 8 Streams API; Java 8 Streams API intermediate Operation; Java 8 Streams map, filter, flatMap, distinct, sorted, peek, limit, skip function with example "
date: 2021-04-01T00:06:41+05:30
weight: 6
tags : ["Exploring Java 8"]
---

Hello there techie Ninja, Hope you are doing good. Welcome to the Blog. 
Today in this blog we would be exploring some intermediate operations that can be performed on streams.
I hope you went through my last blog "Introduction to Streams API". If you are a beginner to java streams I would recommend you to read the previous blog, 
which would clear the basics about streams.

## Following are the Intermediate operations available on Streams
* map
* filter
* flatMap
* distinct
* sorted
* peek
* limit
* skip

Before we dig into those operations, I would like to tell you that, each of those methods returns a new Stream 
reference. Each of the intermediate operations creates a new streams object, that's why you can take advantage of 
method chaining and can write concise and easy-to-read code.


### map() operation
map() operation accepts a function Functional Interface Lamdba and it perform transform operation. use case of map() operation would be whenever you wanted to transform any data from one data type to another data type you can use the map operation.

Syntax :
```java
Stream<R> map(Function<? super T, ? extends R> mapper)
```

Example :
```java
package org.java4ninja.exploringjava8.streams;

import java.util.Arrays;
import java.util.List;
import java.util.function.Function;
import java.util.stream.Collectors;

class Employee {
    private String firstName;
    private String lastName;

    public Employee(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    @Override public String toString() {
        return "Employee{firstName='" + firstName + "\' lastName='" + lastName + "\'}";
    }
}

public class StreamsIntermediateExample {
    public static void main(String args[]) {
        List<String> listOfEmployeeNames = Arrays.asList("Will Smith", "Smith Jones",
            "Cierra Vega", "Alden Cantrell", "Thomas Crane");

        // function which input the string and convert the string to the object of Employee
        Function<String, Employee> createEmployeeFromName = (name) -> {
            String firstName = name.split(" ")[0];
            String lastName = name.split(" ")[1];
            return new Employee(firstName, lastName);
        };

        List<Employee> listOfEmployeeObjects = listOfEmployeeNames
            .stream()
            .map(createEmployeeFromName)
            /* more concise way of writing
            /*.map((name) -> {
                String firstName = name.split(" ")[0];
                String lastName = name.split(" ")[1];
                return new Employee(firstName, lastName);
            })*/
            .collect(Collectors.toList());

        System.out.println(listOfEmployeeObjects);
    }
}
```
<br/>

### filter() Operation
filter() operation is used to filter out elements from the streams. suppose you have streams of elements, and you want that certain element which does not match our criteria should be filtered out. and should not be sent ahead for further processing. in such cases, you can use filter() operation. filter operation accepts a Predicate function as an Input.

Syntax :
```java
Stream<T> filter(Predicate<? super T> predicate);
```

Example :
```java
package org.java4ninja.exploringjava8.streams;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

class Student {
    private String name;
    private double marksOutOf100;

    public Student(String name, double marksOutOf100) {
        this.name = name;
        this.marksOutOf100 = marksOutOf100;
    }

    public String getName() {
        return name;
    }

    public double getMarksOutOf100() {
        return marksOutOf100;
    }
}

public class FilterOperationOnStreams {
    public static void main(String args[]) {

        List<Student> studentList = Arrays.asList(
            new Student("Tim", 86),
            new Student("Joe", 44),
            new Student("Lisa", 91),
            new Student("Tom", 52));

        Predicate<Student> passingCriteria = student -> student.getMarksOutOf100() > 50;

        List<String> listOfStudentWhoPassTheExam = studentList
            .stream()
            .filter(passingCriteria)
            /* more concise way inline predicate function */
            //.filter(student -> student.getMarksOutOf100() > 50)
            .map(student -> student.getName())
            /* more concise way using method reference */
            //.map(Student::getName)
            .collect(Collectors.toList());

        System.out.println(listOfStudentWhoPassTheExam);
    }
}
```
<br/>

### flatMap() Operation
flatMap() is the most confusing operation of streams, Many people gets confuse between map() and flatMap(). It would be easy if you understood them correctly with the correct example. flatMap() operation is used to convert the Streams<Streams<T>> to Streams<T>. basically, if you have a List of elements X, and inside each element, you have another elements Y, in order to create a list of all elements of Y. you can use flatMap(), if you use map() you will end up getting List<List<Y>>.

Syntax :
```java
Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
```

Example :
```java
package org.java4ninja.exploringjava8.streams;

import java.util.List;
import java.util.stream.Collectors;
import lombok.AllArgsConstructor;
import lombok.Data;
import static java.util.Arrays.asList;

/* I have used lombok here in order to write minimal code,You can
always use lombok, or manually create constructor and getter setters*/
@Data @AllArgsConstructor
class Sales {
    private String item;
    private double price;
}

@Data @AllArgsConstructor
class SalesEmployee {
    private String name;
    private List<Sales> listOfSales;
}

public class FlatMapOperationOnStreams {
    public static void main(String args[]) {
        List<SalesEmployee> listOfEmployee = asList(
            new SalesEmployee("Joe", asList(
                new Sales("Iron", 150),
                new Sales("Washing Machine", 1500))),
            new SalesEmployee("Honey", asList(
                new Sales("Mobile", 1400),
                new Sales("TV", 13499))),
            new SalesEmployee("Lisa", asList(
                new Sales("Bed", 150),
                new Sales("Laptop", 12999))),
            new SalesEmployee("Hayaat", asList(
                new Sales("Pen Drive", 399),
                new Sales("Desktop Computer", 8999))));

        List<Sales> listOfSales = listOfEmployee
            .stream()
            .flatMap(employee -> employee.getListOfSales().stream())
            .collect(Collectors.toList());

        System.out.println(listOfSales);
    }
}
```
<br/>

### distinct() and sorted()
The name itself depicts its usage, distinct() method used to extract out all the distinct elements from the streams. and remove all duplicate elements. distinct does not take any argument as input.

sorted() method is useful to sort the elements in the streams. it sorts the element in its natural order. It expects that the class should be comparable. otherwise, you can pass a Comparator object to the sorted() method.

Syntax :
```java
Stream<T> distinct();
Stream<T> sorted();
Stream<T> sorted(Comparator<? super T> comparator);
```

Example :
```java
package org.java4ninja.exploringjava8.streams;

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

public class DistinctAndSortedOnStreams {
    public static void main(String args[]) {

        List<Integer> listOfIntegers = Arrays.asList(1, 2, 5, 33, 22, 2,
            33, 90, 11, 15, 19, 8, 90, 155, 65, 22);

        List<Integer> listOfDistinctAndSortedIntegers = listOfIntegers
            .stream()
            .distinct()
            .sorted()
            .collect(Collectors.toList());
        System.out.println(listOfDistinctAndSortedIntegers);

        List<Integer> listOfDistinctAndSortedInReverseOrderIntegers = listOfIntegers
            .stream()
            .distinct()
            .sorted(Comparator.reverseOrder())
            .collect(Collectors.toList());
        System.out.println(listOfDistinctAndSortedInReverseOrderIntegers);
    }
}
```
<br/>


### peek() limit() and skip()
peek() operation is more of debugging method in the streams, it accepts a consumer function and generally, we use it for printing things out so that on each step of streams you will come to know which elements are going to pass to the next intermediate operation

limit() operation is used to limit the number of the element which can be pass through the streams. it accepts a Long integer, which used to denote the number of the element should be pass to the next intermediate operations

skip() operation again is used to skip first n elements from the streams. it accepts Long integer which is used to skip the first n elements from the beginning of the streams

Syntax :
```java
Stream<T> peek(Consumer<? super T> action);
Stream<T> limit(long maxSize);
Stream<T> skip(long n);
```

Example :
```java
package org.java4ninja.exploringjava8.streams;

import java.util.Arrays;
import java.util.stream.Collectors;

public class PeekLimitAndSkipOperation {
    public static void main(String args[]) {
        Arrays
            .asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
            .stream()
            // skip first 2 element
            .skip(2)
            // limit the result for next 5 elements only
            .limit(5)
            // do a peek a boo on the current elements of streams
            .peek((element) -> System.out.println(element))
            .peek(System.out::println)
            .collect(Collectors.toList());
    }
}
```


I hope you learned all the operations we have discussed in this blog, Let me know your comment, suggestion for this blog. it would be helpful for me to improve myself and my blogs.

Till then,
Thank You
Good-Bye
Happy Coding



[<< Chapter 5 : Introduction to Streams API ](/exploringjava8/chapter5/) | [Chapter 7 : Internal working of Streams  >> ](/exploringjava8/chapter7/)
