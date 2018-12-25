---
layout: blogpost
title: "Spring Boot Hibernate Tips"
categories: java spring hibernate
excerpt: "A tutorial on how to create a Web Application using Spring Boot & Hibernate"
---

## 1. Overview

Hibernate needs no introduction. It is the most popular ORM out there for Java.

Similarly, Spring Boot is the most powerful, and easy to use framework out there for Java.

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

## 3. Entities

In this example, we'll use JPA to create universities and students.

It's always a good idea to store common logic & properties in a superclass.

We will create a superclass for our entities and store common properties in it.

Let's have a look at our BaseEntity class.

<script src="https://gist.github.com/mohitsinha/bfb4a195cdbdeef883f2b2525458dcc1.js"></script>

One thing you can notice is that I haven't used the `@Data` annotation  from Lombok on our class.
`@Data` annotation automatically adds `@ToString` annotation which may cause Stack Overflow errors.
Hence it's better to manage the annotations manually.

`@MappedSuperclass` annotation allows entities to inherit properties from a base class.
This annotation is very important if you want to inherit properties from a base class.

`@EntityListeners({AuditingEntityListener.class})` enables Auditing. We are using `@CreatedDate` & `@LastModifiedDate` to capture when the entity was created or modified. This will be taken care by Spring Data JPA.

`@JsonIdentityInfo` will avoid the Stack Overflow errors when converting our entities to JSON.  
This annotation is required to break the infinite cycle due to the bi-directional relationships between our entities. 

You may also want to check out `@JsonBackReference` & `@JsonManagedReference` to see if they fit better with your requirements.

Let's have a look at our University & Student entities.

<script src="https://gist.github.com/mohitsinha/10c13b0e5cf253f53a41a3dd4c9d810c.js"></script>

<script src="https://gist.github.com/mohitsinha/87558ff5ab42e4a29631973355dd8665.js"></script>

`@Audited` will enable Hibernate managing the auditing (tracking changes) on that Entity.

## 4. Coniguration

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
