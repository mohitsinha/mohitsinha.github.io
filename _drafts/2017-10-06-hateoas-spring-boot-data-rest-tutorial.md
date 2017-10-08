---
layout: blogpost
title: "HATEOAS With Spring Boot Data Rest Tutorial"
categories: java spring
excerpt: "A tutorial on how to create a Hypermedia Driven REST Service using Spring Boot"
---

HATEOAS (Hypermedia as the Engine of Application State) specifies that the REST API's 
should provide enough information to the client to interact with the server. 
This is different from the SOA (Service-Oriented Architecture) where a client 
and a server interact through a fixed contract. We'll look more into HATEOAS in a while.

Spring Data Rest is built on top of Spring Data and Spring Web MVC. 
It analyzes all the domain models and exposes Hypermedia Driven REST endpoints 
for them automatically. In the meanwhile, all the features of Spring Data 
like sorting, pagination etc. are available in these endpoints. 

We'll see with the help of a very simple example how to implement this.

## Dependencies

We'll use Gradle to build our project.