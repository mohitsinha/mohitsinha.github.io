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

## 4. Stock Market Example

In this example, we'll simulate stock market transactions and notify the client about them.

The steps involved are:

 - Initialize stocks with their prices
 - After every second update prices of stock and make stock transactions.
 - The client will receive the details of the stock transactions after every second.

Let's have a look at the Stock class.

<script src="https://gist.github.com/mohitsinha/975122beb80000efc122f1c4232d6fa8.js"></script>

We'll initialize some random Stocks at the start of the application.

Let's have a look at the StockTransaction class.

<script src="https://gist.github.com/mohitsinha/b7a46782aefdc3a4cfa7589dd5bd631b.js"></script>

## 5. Reactive SSE

We'll create a service which returns a `Flux` of StockTransactions.

<script src="https://gist.github.com/mohitsinha/952e69b8101d0446a82ead544d9ba1fd.js"></script>

We are creating a `Flux` which generates a long value every second. We are then updating the price of every stock.

We are creating another `Flux` which creates a StockTransaction.

The `Flux.zip` method is taking both these `Flux` and combining them.

They'll be combined in a strict sequence (when both `Flux` have emitted their nth item).

We are then returning the second `Flux`.

Hence, the resulting `Flux` will emit a StockTransaction after every second.

## 5. Web API

We'll create an endpoint __GET /stock/transaction__ which will continuously fetch details of the latest transactions happening.

<script src="https://gist.github.com/mohitsinha/2483fe90c1562378055096b6334f5144.js"></script>

`MediaType.APPLICATION_STREAM_JSON_VALUE` signifies that the server will send SSE.

This API will return a response with the header `Content-Type: application/stream+json`.

The cURL commands for testing this is <br/>`curl -v http://localhost:8080/stock/transaction`.

The output obtained will look like this

<script src="https://gist.github.com/mohitsinha/5cd9bfc2a5779b010e525f9f06d54904.js"></script>

## 6. Conclusion

I have tried explaining, with a simple example, how to send SSE using Spring Boot.

You can read more about:

 - [Project Reactor](https://projectreactor.io/)

You can find the complete example on [Github](https://github.com/mohitsinha/spring-boot-reactive-sse).

