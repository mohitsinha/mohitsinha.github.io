---
layout: blogpost
title: "Introduction to Spring Cloud - Config (Part I)"
categories: java spring cloud
excerpt: "A tutorial on how to create a Hypermedia Driven REST Service using Spring Boot"
---

Profiles, 
 Use profiles to describe beans and bean graphs that change from one environment to another. You can activate one or more profiles at a time. Beans that do not have a profile assigned to them are always activated. Beans that have the profile default are activated only when there are no other active profiles.
 @Profile
 
 Profiles let you describe sets of beans that need to be created differently in one environment versus another. You might, for example, use an embedded H2 javax.sql.DataSource in your local dev profile, but then switch to a javax.sql.DataSource for PostgreSQL

refresh scope

curl -X POST \
  http://localhost:8080/actuator/refresh \
  -H 'content-type: application/json' \
  -d '{}'
  
  
spring.cloud.config.server.git.username
spring.cloud.config.server.git.password
spring.cloud.config.uri

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
like MongoDB, MySQL etc. The full list of supported databases is given 
[here](https://docs.spring.io/spring-data/rest/docs/current/reference/html/#getting-started.bootstrap).

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
