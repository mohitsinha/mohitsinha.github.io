---
layout: blogpost
title: "Introduction to HATEOAS With Spring Boot Data Rest"
categories: java spring
excerpt: "A tutorial on how to create a Hypermedia Driven REST Service using Spring Boot"
---

HATEOAS (Hypermedia as the Engine of Application State) specifies that the REST API's 
should provide enough information to the client to interact with the server. 
This is different from the SOA (Service-Oriented Architecture) where a client 
and a server interact through a fixed contract. We'll look more into HATEOAS in a while.

Spring Data Rest is built on top of Spring Data, Spring Web MVC & Spring Hateos. 
It analyzes all the domain models and exposes Hypermedia Driven REST endpoints 
for them automatically. In the meanwhile, all the features of Spring Data Repositories
like sorting, pagination etc. are available in these endpoints. 

We'll see with the help of a very simple example how to implement this.

## Dependencies

We'll use Gradle to build our project.

<script src="https://gist.github.com/mohitsinha/63bbced7d613c88913ffcbe8cf835054.js"></script>

We'll use H2 to run our project. The same concept can be applied for different databases 
like MongoDB, MySQL etc. The full list of supported databases is given [here](https://docs.spring.io/spring-data/rest/docs/current/reference/html/#getting-started.bootstrap).

## Spring Data Rest

In this example, we'll use JPA to create cities and countries.

Let's have a look at our Country class.

<script src="https://gist.github.com/mohitsinha/5133fa2253f56303c817e1247173bc99.js"></script>

Let's have a look at our City class.

<script src="https://gist.github.com/mohitsinha/32d1d95bcd90400c980ea70b7968269d.js"></script>

As we are using JPA in our project, we are creating an association between a City and a Country.