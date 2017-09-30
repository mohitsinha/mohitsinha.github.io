---
layout: blogpost
title: "Reactor Tutorial"
categories: java
excerpt: "A tutorial on how to use the various features of Reactor-Core for reactive programming"
---

Reactive programming is about building asynchronous, non-blocking, and 
event-driven applications that can easily scale.

Reactor is a Reactive library for building non-blocking applications. 
It is based on the Reactive Streams Specification. 
Java 8 is required to use this library. It is integrated into Java 9.

Reactive Streams are push-based. 
It is the `Publisher` that notifies the `Subscriber` of newly available values as they come, and this push aspect is key to being reactive. 

## Dependencies

We'll need reactor-core and reactor-test along with JUnit to go through this tutorial.

<script src="https://gist.github.com/mohitsinha/f73a7358e5a23567bd039970e25ad5f5.js"></script>

## Publishers (Mono & Flux)

`Mono` and `Flux` are implementations of the `Publisher` interface. 
A `Flux` will observe 0 to N items and eventually terminate successfully or not. 
A `Mono` will observe 0 or 1 item, with `Mono<Void>` hinting at most 0 items.

Let's see with the help of tests how to use this library.

<script src="https://gist.github.com/mohitsinha/8b49633f768a351239a48f21de37de02.js"></script>

In this example, we created an empty `Mono` and a `Flux` and used a 
`StepVerifier` to test them. The `Publisher`s completed without emitting any object.

<script src="https://gist.github.com/mohitsinha/9650e314c6535e45212760f73c7f7172.js"></script>

We initialized the `Mono` & `Flux` in different ways and verified that they are 
emitting the expected objects.

<script src="https://gist.github.com/mohitsinha/a091a64541e1188794b9fdd9ca4ee2af.js"></script>

We can use all the Java 8 Stream operations on `Mono` & `Flux`. 

In the first example, we mapped a `Mono` emitting a name to a `Mono` 
emitting the same name in lower-case. We verified that the resulting `Mono` emitted 
the same name in lower-case.

In the second example, we mapped a `Flux` emitting names to a `Flux` emitting the names 
in lower-case after applying a filter that passed only names starting with 'k'. 
We verified that the resulting `Flux` emitted only names starting with 'k' in lower-case.


<script src="https://gist.github.com/mohitsinha/a771ac374e48264760c3605273daaf37.js"></script>

In the first example, we have 3 `Flux`es emitting the title, first name and the last name. 
`Flux.zip` is combining them in a strict sequence (when all `Flux`es have emitted their _nth_ item). 
We then concatenated them to create a `Flux` emitting the full names.

In the second example, we created a `Flux` that generates a long value every _5 ms_. 
We then combined it with the `Flux` firstNames. Hence, the resulting `Flux` 
will emit a value after every _5 ms_. We used this `Flux` similarly as in the previous 
example and verified that the sequence of combination is maintained despite the delay.

<script src="https://gist.github.com/mohitsinha/19179628004cdf36e738a00c9a83990b.js"></script>

Interleaving is a concept in which data is written non-sequentially to improve performance.

We have two `Flux`es, one of them emitting values with a delay. 
`Flux.mergeWith` merges them into an interleaved sequence. 
Hence, we see that the sequence has changed.

`Flux.concatWith` merges them into a non-interleaved sequence. 
Hence, we see that the sequence remains the same despite the delay.

<script src="https://gist.github.com/mohitsinha/e7f22d658586f45bac4614a044e93b49.js"></script>

## Conclusion

You can read more about [Project Reactor](https://projectreactor.io/docs/core/release/reference/docs/index.html).

To learn how to create Reactive applications using Spring Boot And Reactor you can see this tutorial.

 - 
 - 
