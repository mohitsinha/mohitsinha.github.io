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
It is the Publisher that notifies the Subscriber of newly available values as they come, and this push aspect is key to being reactive. 

## Dependencies

We'll need reactor-core and reactor-test along with JUnit to go through this tutorial.

<script src="https://gist.github.com/mohitsinha/f73a7358e5a23567bd039970e25ad5f5.js"></script>

## Publishers (Mono & Flux)


