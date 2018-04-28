---
layout: blogpost
title: "Introduction to Spring Cloud - Config (Part I)"
categories: java spring cloud
excerpt: "A tutorial on how to manage configurations for microservices using Spring Cloud."
---

## 1. Overview

Spring Cloud provides tools for developers to quickly build some of the common patterns in 
distributed systems (e.g. configuration management, service discovery, circuit breakers, intelligent routing, micro-proxy, control bus, one-time tokens, global locks, leadership election, distributed sessions, cluster state). 

It helps manage the complexity involved in building the distributed system.

In this tutorial series, we'll be using some of these patterns. 

## 2. Microservices

Microservices is a software development architectural style, that decomposes the application
into a collection of loosely coupled services.

It improves modularity, thus making the application easier to develop, test & deploy.

It also makes the development process more efficient by parallelizing small teams to work on
different services.

There are also various difficulties regarding communication between services, managing configurations, etc 
in a microservice architecture.

One should go through the [Twelve-Factor App Manifesto](https://12factor.net/) to solve many of the problems
arising with a Microservice architecture.

## 3. Spring Cloud Config

Spring Cloud Config provides server and client-side support for externalized configuration 
in a distributed system.

It has two components, the Config Server & the Config Client.

The Config Server is a central place to manage external properties for applications across all environments.
We could also version the configuration files using Git. It exposes REST API's for clients to connect
and get the required configuration. We can also leverage [Spring Profiles](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html)
to manage different configuration files for different profiles (environments).

The Config Client binds to the Config Server and initializes Spring `Environment` with remote property sources.

## 4. Dependencies

We'll use Gradle to build our projects. I recommend using [Spring Initializr](http://start.spring.io/) for bootstrapping your projects.

For the Config Server we'll use:

 - Spring Boot 2
 - Spring Cloud Config Server

<script src="https://gist.github.com/mohitsinha/ea099e1974f05473f58371cd7fbef19a.js"></script>

For the Config Client we'll use:

 - Spring Boot 2
 - Spring Boot Actuator
 - Spring Boot Webflux
 - Spring Cloud Starter Config

<script src="https://gist.github.com/mohitsinha/5949f5974790aed8229bb50bbc046aa7.js"></script>

## 4. Auto-Configuration

We'll leave Spring Boot to automatically configure our application based on the dependencies added.

<script src="https://gist.github.com/mohitsinha/45436bc4503b3be888e6de39fc3c9210.js"></script>
   
For using non-default values in our application configuration, we can specify them as properties and Spring Boot will automatically use them to create beans.

<script src="https://gist.github.com/mohitsinha/d1335db9653e98bba9c407258adabb5a.js"></script>

All beans necessary for MongoDB, Web, and Security will be automatically created.

## 5. Database

We'll be using MongoDB in our example and a simple POJO. A `PersonRepository` bean will be created automatically.

<script src="https://gist.github.com/mohitsinha/ee644471ba1a22c0ec8c553937e69976.js"></script>

## 6. Web API

We’ll create REST endpoints for `Person`.

Spring 5 added support for creating routes functionally while still supporting the traditional annotation-based way of creating them.

Let’s look at both of them with the help of examples.

### 6.1. Annotation-based

This is the traditional way of creating endpoints.

<script src="https://gist.github.com/mohitsinha/f0bea8acd69874956f9b1353208522fe.js"></script>

This will create a REST endpoint __/person__ which will return all the `Person` records reactively.

### 6.2. Router Functions

This is a new and concise way of creating endpoints.

<script src="https://gist.github.com/mohitsinha/1fcd569a1f8e6696edd1ab85f109b4b1.js"></script>

The `nest` method is used to create nested routes, where a group of routes share a common path (prefix), 
header, or other `RequestPredicate`.

So, in our case all the corresponding routes have the common prefix __/person__.

In the first route, we have exposed a GET API __/person/{id}__ which will retrieve the corresponding record and return it.

In the second route, we have exposed a POST API __/person__ which will receive a Person object and save it in the DB.

The cURL commands for the same:

<script src="https://gist.github.com/mohitsinha/f1d4709c84484586cb7dc9434af2e230.js"></script>

We should define the routes in a Spring configuration file.

## 7. Security

We'll be using a very simple basic authentication mechanism in our example.

<script src="https://gist.github.com/mohitsinha/ddba3f489cc57e625afd25199b81d54e.js"></script>

We have added some users for our application and assigned different roles to them.

## 8. Conclusion

I have tried explaining, with a simple example, how to build a simple Reactive web application using Spring Boot.

You can read more about:

 - [Spring Cloud](https://projects.spring.io/spring-cloud/spring-cloud.html)
 - [Spring Data Reactive](https://spring.io/blog/2016/11/28/going-reactive-with-spring-data)
 - [Spring Functional Web Framework](https://spring.io/blog/2016/09/22/new-in-spring-5-functional-web-framework)

You can find the complete example for the [Config Server](https://github.com/mohitsinha/tutorials/tree/master/config-server) & [Library Service](https://github.com/mohitsinha/tutorials/tree/master/library-service) on Github.


