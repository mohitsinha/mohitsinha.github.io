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

We'll look into some common errors that you may run into when using them together and how to fix them.

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

## 4. Configuration

Let's check the configurations required to run our project.

### 4.1. Hibernate Jackson Module

<script src="https://gist.github.com/mohitsinha/8ff16cf1e9876fddf9d08b9302530b73.js"></script>

We are registering a new Jackson Module.   
Spring Boot will auto-detect it and register it to the `ObjectMapper` bean.

### 4.2. Hibernate Envers

To enable Envers auditing, we have to extend our Repositories with `RevisionRepository`.

Let's have a look at our `UniversityRepository`.

<script src="https://gist.github.com/mohitsinha/0026d1b9599315996b1dc96c7bb3cce5.js"></script>

We'll have to similarly do this for our `StudentRepository`.

<script src="https://gist.github.com/mohitsinha/17ef59d581502218104e89f074e935db.js"></script>

We also have to annotate our main class with `@EnableJpaRepositories(repositoryFactoryBeanClass = EnversRevisionRepositoryFactoryBean.class)`.

We'll look at the main class in a while after going through other annotations that we need.

### 4.3. Spring Data Auditing

To enable this, we'll have to annotate our main class with `@EnableJpaAuditing`.

Let's look at it.

<script src="https://gist.github.com/mohitsinha/596151dbff1bc00159782477912c27d5.js"></script>

## 5. Conclusion

I have tried explaining, with a simple example, how to create REST applications using Spring 
Boot & Hibernate. 

This might solve some of your Stack Overflow errors.  
Else, you might want to consider writing your own Data Transfer Objects (DTO).

If you need support for data types that aren't supported by the core Hibernate ORM, you might want to check out this [library](https://github.com/vladmihalcea/hibernate-types).

You can read more about:

 - [Spring Data JPA](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)

You can find the complete example on [Github](https://github.com/mohitsinha/tutorials/tree/master/hibernate-tips).
