---
layout: blogpost
title: "Spring Boot Hibernate Tips"
categories: java spring hibernate
excerpt: "A tutorial on how to create a Web Application using Spring Boot & Hibernate"
---

## 1. Overview

Hibernate needs no introduction. It is the most popular ORM out there for Java.

This tutorial isn't about Hibernate or Spring Boot, there are tons of them out there.
We'll look into some common mistakes that you may make when using them together and how to fix them.

## 2. Dependencies

We'll use Gradle to build our project. I recommend using [Spring Initializr](http://start.spring.io/) for bootstrapping your project.

We'll use:

 - Spring Boot 2
 - Spring Webflux
 - Spring Data Jpa
 - Spring Data Envers
 - Jackson Annotations
 - Jackson DataType Hibernate
 - H2 Database
 - Lombok

Spring Data Envers allows us to access entity revisions managed by Hibernate Envers.

Jackson Annotations will help us to avoid the common Stack Overflow errors that are caused by JPA relationships.

Jackson DataType Hibernate Module will help with Hibernate types and lazy-loading aspects.

We'll look into all of these in a while.

<script src="https://gist.github.com/mohitsinha/361a23f1d275d030ba8a3c4061ad06ee.js"></script>

We'll use H2 to run our project.

## Spring Data Rest

In this example, we'll use JPA to create cities and countries.

Let's have a look at our Country class.

<script src="https://gist.github.com/mohitsinha/5133fa2253f56303c817e1247173bc99.js"></script>

Let's have a look at our City class.

<script src="https://gist.github.com/mohitsinha/32d1d95bcd90400c980ea70b7968269d.js"></script>

As we are using JPA in our project, we are creating an association between a City and a Country. 
Many Cities can be associated with a Country.

Let's create the repositories for them.

<script src="https://gist.github.com/mohitsinha/ecfa9307f01184cf04343bc818abf6a1.js"></script>

This will create a repository for Country and also expose the REST endpoints (GET, POST, PUT, DELETE, PATCH) for the same. 
As `JPARepository` extends `PagingAndSortingRepository`, paging & sorting functionality will be automatically added for the GET endpoint. 
By default, the path is derived from the uncapitalized, pluralized, simple class name of the domain class being managed. 
In our case, the path will be _countries_.

<script src="https://gist.github.com/mohitsinha/4909bfffdb1261776f054da98ce56aef.js"></script>

We have customized the path to _metropolises_.

## HATEOAS

Let's check the APIs after we run our project.

`curl 'http://localhost:8080'`

<script src="https://gist.github.com/mohitsinha/45ef59569d763f0f39aac0e9bfc993a0.js"></script>
   
We get some information about the available APIs. We can further explore about the metadata by 
hitting the _profile_ API. You can read more about the metadata 
[here](https://docs.spring.io/spring-data/rest/docs/current/reference/html/#metadata).

Let's add a few Countries.

<script src="https://gist.github.com/mohitsinha/03ffa6c89f1f50d41fadd65546f6cfc6.js"></script>

Let's fetch a paginated result of Countries with the results sorted by Country name, the 
page size 2 and the 1st page.

`curl 'http://localhost:8080/countries/?sort=name,asc&page=1&size=2'`

<script src="https://gist.github.com/mohitsinha/31a61a516e4cc1edcc2115a69af0de9a.js"></script>

Apart from the expected countries, we also get the links to different pages and 
further information that might help in handling pagination better. 

The links to the first, previous, self, next and last pages can directly be used.

Let's add a City and associate it with a Country.

`curl 'http://localhost:8080/metropolises' -X POST -d '{"name":"Osaka", "country":"http://localhost:8080/countries/1"}' -H 'Content-Type: application/json'`

We have to pass the url of the country and this will be mapped to Japan. 
We saved Japan first, hence its id is 1.

Let's see what we get when we fetch that City.

`curl 'http://localhost:8080/metropolises/1'`

<script src="https://gist.github.com/mohitsinha/73ca2d35dee35cb6e2d0e614fd34b1b1.js"></script>

We are getting a link to the Country associated with it. 
Let's see what we get in response for it. 

<script src="https://gist.github.com/mohitsinha/84be4347541b040e1e3b44826e577131.js"></script>

## Conclusion

I have tried explaining, with a simple example, how to create REST applications using Spring 
Data Rest. You can read more about setting up policies and integrating with Spring Security 
[here](https://docs.spring.io/spring-data/rest/docs/current/reference/html/#security).

You can find the complete example on [Github](https://github.com/mohitsinha/tutorials/tree/master/hateoas-spring-data-rest-example).
