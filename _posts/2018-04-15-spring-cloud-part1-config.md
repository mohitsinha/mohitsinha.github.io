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

It has two components, the __Config Server__ & the __Config Client__.

The Config Server is a central place to manage external properties for applications across all environments.
We could also version the configuration files using Git. It exposes REST API's for clients to connect
and get the required configuration. We can also leverage [Spring Profiles](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html)
to manage different configuration files for different profiles (environments). 

For eg: we may decide to use an embedded H2 in our local dev profile, but use PostgreSQL in our prod profile.

The Config Client binds to the Config Server and initializes Spring `Environment` with remote property sources.

## 4. Dependencies

We'll use Gradle to build our projects. I recommend using [Spring Initializr](http://start.spring.io/) for bootstrapping your projects.

### 4.1. Config Server

We'll use:

 - Spring Boot 2
 - Spring Cloud Config Server

<script src="https://gist.github.com/mohitsinha/ea099e1974f05473f58371cd7fbef19a.js"></script>

### 4.2. Config Client

We'll use:

 - Spring Boot 2
 - Spring Boot Actuator
 - Spring Boot Webflux
 - Spring Cloud Starter Config

<script src="https://gist.github.com/mohitsinha/5949f5974790aed8229bb50bbc046aa7.js"></script>

## 5. Auto-Configuration

We'll leave Spring Boot to automatically configure our application based on the dependencies added
and the properties specified.

### 5.1. Config Server

<script src="https://gist.github.com/mohitsinha/7096508480a9633050ac513738cd6deb.js"></script>

We'll also have to specify the Git Repository where the configurations are stored.

<script src="https://gist.github.com/mohitsinha/1cf59b97f546ac2002f9ad695d3a92d6.js"></script>

`spring.cloud.config.server.git.uri` specifies the Git Repository where the configurations are stored.

You can also pass the user credentials to access the Repository by passing the username & password.
`spring.cloud.config.server.git.username
 spring.cloud.config.server.git.password
`  

### 5.2. Config Client

<script src="https://gist.github.com/mohitsinha/c59ffa24b3a41f2d7fb6918d8d8720a1.js"></script>

<script src="https://gist.github.com/mohitsinha/18f6e72bdafbe29cb313a4dde1856716.js"></script>

`spring.application.name` is used to fetch the correct configuration file from the Git Repository.
It's a very important property when used with Spring Cloud projects. 
We'll see this later in this tutorial series.

The bootstrap properties are added with higher precedence, hence they cannot be overridden
by local configuration. You can read more about it [here](https://cloud.spring.io/spring-cloud-static/spring-cloud-commons/2.0.0.M9/multi/multi__spring_cloud_context_application_context_services.html#_the_bootstrap_application_context).

<script src="https://gist.github.com/mohitsinha/285c7cf1773fca124ae191c071210f30.js"></script>   

`management.endpoints.web.exposure.include=refresh` exposes the refresh actuator endpoint. 
We'll look at it in a while.


## 6. REST API

Let’s look at some of the REST API's that are automatically created to manage and monitor 
these services.

### 6.1. Config Server

Let's look at the configuration values for the application __libary-service__.

`curl http://localhost:8888/library-service/default`

The output obtained will look like this

<script src="https://gist.github.com/mohitsinha/ffeff49c82e50fa26d25a97623be6576.js"></script>

It gives details about the Git Repository for the configuration files, the configuration values, etc.
Here we can see the value for the property _library.name_. 

### 6.2. Config Client

We'll add a web endpoint that will use the property _library.name_ defined in the 
configuration in the Git Repository.

<script src="https://gist.github.com/mohitsinha/dcf906cb5f9d7710552dcb857ddf650a.js"></script>

A Bean marked with the annotation `RefreshScope` will be recreated when a configuration change occurs
and a `RefreshScopeRefreshedEvent` is triggered.

Whenever a configuration change occurs in the Git Repository, we can trigger a `RefreshScopeRefreshedEvent`
by hitting the Spring Boot Actuator Refresh endpoint. The refresh endpoint will have to be enabled.

`management.endpoints.web.exposure.include=refresh` 

The cURL command for the Actuator Refresh endpoint:

```
curl -X POST 
  http://localhost:8080/actuator/refresh \
  -H 'content-type: application/json' \
  -d '{}'
```

This will update the configuration values on the Config Client.

We can now check the endpoint and verify if the new configuration value is being reflected or not.

`curl http://localhost:8080/details`

What if there are multiple instances of the Client running, and we want to refresh the configuration values
in all of them? 

This can be achieved by using __Spring Cloud Bus__. 
It links the nodes of a distributed system with a lightweight message broker.
You can read more about it [here](https://cloud.spring.io/spring-cloud-static/spring-cloud-bus/1.2.1.RELEASE/).

## 7. Conclusion

I have tried explaining, with a simple example, how to manage externalized configurations using Spring Cloud Config.
In the next tutorial, we'll look at __Service Discovery__.

You can read more about:

 - [Spring Cloud](https://projects.spring.io/spring-cloud/spring-cloud.html)

You can find the complete example for the [Config Server](https://github.com/mohitsinha/tutorials/tree/master/config-server) & [Library Service](https://github.com/mohitsinha/tutorials/tree/master/library-service) on Github.


