---
layout: blogpost
title: "Spring Boot Server-Sent Events Tutorial"
categories: java spring
excerpt: "A sample reactive application on how to use SSE."
---

## 1.Overview

Spring 5, which will release later this year, will support building asynchronous and Reactive applications.

To know more about the features in Spring 5 and Reactive Programming, you can refer to this [article](https://dzone.com/articles/spring-boot-reactive-tutorial).

I'll highly suggest you to do that.

Spring 5 will make it easier to implement Server-Sent Events (SSE).

We'll see with the help of a simple stock market simulation how to implement SSE.

## 2. Server-Sent Events (SSE)

SSE is a web technology where a browser receives updates from a server via HTTP connection.

It is better than polling because polling has a lot of HTTP overhead.

Unlike WebSockets, it is unidirectional (server to browser).

SSEs are sent over traditional HTTP, hence they don't require any special implementation on the server.

## 3. Dependencies

We'll use Gradle to build our project. I recommend using [Spring Initializr](http://start.spring.io/) for bootstrapping your project.

We'll use:

 - Spring Boot 2
 - Spring Webflux
 - Lombok

Not all the Spring libraries have a stable release yet.

<script src="https://gist.github.com/mohitsinha/7b61995de523f8954d5fe0b63e15e848.js"></script>