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

<script src="https://gist.github.com/mohitsinha/8b49633f768a351239a48f21de37de02.js"></script>

<script src="https://gist.github.com/mohitsinha/9650e314c6535e45212760f73c7f7172.js"></script>

<script src="https://gist.github.com/mohitsinha/a091a64541e1188794b9fdd9ca4ee2af.js"></script>

<script src="https://gist.github.com/mohitsinha/a771ac374e48264760c3605273daaf37.js"></script>

<script src="https://gist.github.com/mohitsinha/19179628004cdf36e738a00c9a83990b.js"></script>

<script src="https://gist.github.com/mohitsinha/e7f22d658586f45bac4614a044e93b49.js"></script>

## Conclusion

You can read more about [Project Reactor](https://projectreactor.io/docs/core/release/reference/docs/index.html).

To learn how to create Reactive applications using Spring Boot And Reactor you can see this tutorial.

 - 
 - 
