---
title: "Chapter 7 : Internal working of Streams"
description: "java 8 streams api; java 8 streams api internal working with graphical representation; step by step working of java 8 streams"
date: 2021-04-01T00:06:45+05:30
weight: 7
tags : ["Exploring Java 8"]
---

Hello there techies, Hope you are doing good. Welcome to Programming4ninja and Exploring Java 8 Series. 
Today in this blog we would be talking about the Java 8 streams. 
it is very essential to understand the way streams work. In order to get the benefit of parallel processing, 
one should be able to understand how element flows in the streams.

Since by now you must have basic knowledge of Streams, I am assuming that you have completed the previous 2 lessons; 
Introduction to Streams and Intermediate operations in Streams

### Can you tell me what is the output of the below code snippet?
```java
public class StreamApiElementOrder {
    public static void main(String args[]) {

        Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
            .stream()
            .peek(i -> System.out.println("before any operation: " + i))
            .filter(i -> i % 2 == 0)
            .map(i -> i * 2)
            .limit(3)
            .collect(Collectors.toList());
    }
}
```
Output :

```text
before any operation: 1
before any operation: 2
before any operation: 3
before any operation: 4
before any operation: 5
before any operation: 6
```

*Shocked !! What happened with 7, 8, 9, and 10? Why they did not get printed?*

Most of you who are familiar with the Streams would know the reason, but others who are learning Streams for the first time 
would not get this. that is why this lesson is important for them.

Stream uses a pull-based approach for processing any stream. 
if you did not write any termination operation in the above example like collect(), you won't get any result in the console. 
Why is it so? because terminal operations are the ones that are requesting elements from streams via pipeline. 
Now You must be questioning what is the pipeline? the pipeline is nothing but the arrangement of all the intermediate operations
before any terminal operation. every single element would get fetch from the stream, by the terminal operators using the pipeline.

So in the above example, terminal operation collect() requested elements from the stream, 
so the first element gets served. While serving you saw the message "before any operation: 1". now on element 1, it applies the filter, since our 
filter is expecting an even number, this element was filtered out from the result. and collector gets no element. 
it requests for the next element, so the 2nd element gets served, and you saw the message "before any operation: 2". 
since 2 is an even number, it gets passed to the next step in the pipeline, i.e. double the number. now the number becomes 4 and it gets passed 
to further intermediate operation i.e. limit(). the limit is tracking the number of elements needed to be passed through it. 
once it found that 3 elements have been pass through it. it will give the end signal to the collector, and the collector will not request more elements.


I believe now you must have understood why 7, 8, 9, and 10 did not get printed. By the time collector requested the 6th data, 
the limit intermediate operation found that they have got the 3 even numbers. and it sends a terminal request to the collector.
and collector did not request any further elements from the stream.

Attaching here a jpeg representation of the streams pipeline for the above example

<img src="/blog-images/stream-pipeline-flow.png" />



Please let me know your suggestion, thoughts, and question. 
This would help me to improve myself and my blog. Till then, 
Thank you Good Bye and Happy Coding



[<< Chapter 6 : Intermediate Streams Operations](/exploringjava8/chapter6/) | [Chapter 8 : Streams terminal functions >> ](/exploringjava8/chapter8/)
