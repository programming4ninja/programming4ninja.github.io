---
title: "Chapter 5 : Introduction to Streams API"
description: "introduction to java 8 streams api with example"
date: 2021-04-01T00:06:39+05:30
weight: 5
tags : ["Exploring Java 8"]
---

Hello there techie ninjas, I hope you are doing well. Today we would be learning one of the most amazing features 
released in Java 8. it provided a lot of benefit for parallel processing of collections and writing code in functional
ways. I would say this is one of the most important chapters of the Java 8 Series, so I would be creating this blog in 
such a fashion that every one of us would understand this functionality.


### Before we start, let's discuss what is Steam in general?
I would say the stream is something that is in motion. 
or something is flowing in unidirectional. It could be Air, Gas or Water. 
You all must have seen a stream of water, it is a continuous flow of water. 
The same is applicable to Data also, if there is data in motion we call it a data stream. 
Innovator of Java 8, saw great potential in this concept. Till java 8, Most of us were using the collection for storing the Data. 
But in most cases, this stored data will eventually be needed to flow between various components of the software. 
There was a lot of common patterns around the collection processing. Innovators of java 8, analyse those and created a Streaming API framework, 
where one could be able to declare flowing data in programming, process the streams, and At the end collect and convert it back to stationary data.


<img src="/blog-images/stationary-vs-stream-data.png" />




With the above diagram, I hope you can conceptually able to visualise streams in java. 
the left-hand side denotes the stationary data stored on the memory, like your collections - list, map, sets and one
on the right-hand side are nothing, but the java streams.



Let's see one example to know how you can get a stream in java

```java
package org.java4ninja.exploringjava8.streams;

import java.util.Arrays;
import java.util.List;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class HelloStreams {
    public static void main(String args[]) {
        // Stationary Data
        List<String> listOfString = Arrays.asList("Apple",
            "Banana", "Orange", "Mango", "Pineapple", "Grapes");

        // Stream made out of Stationary data
        Stream<String> stream = listOfString.stream();

        // Streams of integer from 1 to 100
        IntStream intStreams = IntStream.rangeClosed(1, 100);

        // Streams of different languages 
        Stream<String> someRandomStreams = Stream.of("java", "node", "scala", "bash", "python");
    }
}
```


### Different Stages of Streams
Each and every stream has to pass through different stages of operation given below. We have categorised those in 3 different stages

**Initialisation Phase -> Intermediate Operations -> Terminal Operation**

* The initialisation phase is nothing but creating streams from any collection, any source of data or just starting with a range of integers from 1 to N. In order to use streaming API, you will need the Stream. We will see different ways of initialising streams in the next blog.
* Intermediate Operations are the operations performed on the flowing data. The streams on which this data flow, we call it as a pipeline. We can apply a different intermediate operation on this pipeline, such as filtering elements, converting elements, re-arranging them, skipping element, limiting elements etc.
* We would be having a separate blog on the intermediate operation. The basic intention behind the intermediate operation is to transform the source of data into another useful format.
* We can chain these intermediate operation in order to archive desired output.
* Once we have set up the pipeline, to process data, at the end you need to collect the data in the format you want to project. it can be storing data back in another collection, performing reduce operation, find out count, checking if any element exists in streams.
* The terminal operation will be the endpoint for the stream journey. The stream would be closed and data would be stored back in stationary format. Data will stop flowing on calling terminal operation.

Let's take a simple example to understand these different stages, although we would be exploring each of these stages in a lot of greater details in the next blogs.

```java
package org.java4ninja.exploringjava8.streams;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class HelloStreams {
    public static void main(String args[]) {
        // Stationary Data
        List<String> listOfString = Arrays.asList("Apple", "Banana", "Cat",
            "Dog", "Elephant", "Fish", "Grapes", "Hat", "Ice", "Jacket",
            "Kite", "Lemon", "Mango", "Newspaper", "Orange", "Parrot",
            "Queen", "Rat", "Sun", "Tv", "Umbrella", "Van", "Watch",
            "Xmas", "Zebra");

        // Initialisation stage
        Stream<String> streamsOfString = listOfString.stream();

        // intermediate stages
        Stream<String> intermediateStringStream = streamsOfString
            // intermediate stage of filtering elements
            .filter(s -> s.length() > 3)
            // intermediate stage of convert each element to upper case
            .map(s -> s.toUpperCase())
            // intermediate stage of re-ordering elements
            .sorted(String::compareTo);

        // Terminating streams and collecting all the elements from the stream to list collection
        List<String> finalListOfString = intermediateStringStream
            .collect(Collectors.toList());

        System.out.println(finalListOfString);
    }
}
```


I hope you have got a basic idea behind the Streaming API introduced in java 8. We would be going to talk a lot about the initialisation, intermediate stages and terminal stages in upcoming blogs.

Till then Happy Coding, See you in the next blog.

[<< Chapter 4 : Method & Constructor Reference in Java 8 ](/exploringjava8/chapter4/) | [Chapter 6 : Intermediate Streams Operations >> ](/exploringjava8/chapter6/)
