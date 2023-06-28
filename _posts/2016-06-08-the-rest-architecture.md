---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: The REST Architecture
date:   2016-06-08 9:27:00
tags: general
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

Representational State Transfer (REST) is the architectural style used in the development of Web services for the World Wide Web. RESTful systems typically use HTTP to request, retrieve, and send data to web servers. 

It was designed with several factors in mind:

<ul>
  <li>Performance - does not utilize much bandwidth</li>
  <li>Scalable - able to suppport a large number of components</li>
  <li>Simplistic - non-complex interfaces</li>
  <li>Visibility - communication is seen by service agents</li>
  <li>Portable - send code with data</li>
  <li>Reliable - resistent to failure</li>
</ul>

To meet these demands, REST dictates a list of 6 constraints. 

#### Client-Server

There is a separation of concerns where clients (such as your browser) are not concerned with data storage, and the servers are not concerned with the user interface. This improves the portability of the user interface across multiple platforms, improves scalability by simplifying components, and allows components to evolve independently.

#### Stateless

State session is held in the client. There is no client context that is stored on the server between requests. Each request must contain all the necessary information to process the request. Visibility is improved because a monitoring system does not have to look beyond a single request in order to understand the request. It is reliable due to the ease of recovering from partial failures. And Scalable with the server not having to store data between requests allows to free up resources.

#### Cacheable

Responses must define themselves as cachable or not, to prevent stale or inappropriate data for future requests. Improves efficiency, scalability, and performance by partially or completely removing some interactions.  

#### Uniformed Interface

The uniform interface simplifies and decouples the architecture, which allows each part to evolve independently. REST is defined by four interface constraints: identification of resources, manipulation of resources through representations, self-descriptive messages, and hypermedia as the engine of application state.

#### Layered System

A client cannot typically tell if it is connected to the end server or an intermediary along the way. This improves system scalability by providing load balacing with shared caches, limits overall system complexity, and promtes substrate independence.

#### Code On Demand

Servers can temporarily extend or customize the view, by transfering execuateable code such as Java applets, and JavaScript. An optional contraint used if the architecture gains in benefit. 

For more information, Roy Fielding's doctoral dissertation is hosted on [University of California, Irvine](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm).





