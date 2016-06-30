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

There is a separation of concerns where clients (such as your browser) are not concerned with data storage, and the servers are not concerned with the user interface. 

#### Stateless

State session is held in the client. There is no client context that is stored on the server between requests. 

#### Cacheable

Responses must define themselves as cachable or not, to prevent stale or inappropriate data for future requests.

#### Uniformed Interface

The uniform interface simplifies and decouples the architecture, which allows each part to evolve independently.

#### Layered System

A client cannot typically tell if it is connected to the end server or an intermediary along the way, that improves system scalability by providing load balacing with shared caches.

#### Code On Demand

Servers can temporarily extend or customize the view, by transfering execuateable code such as Java applets, and JavaScript.

For more information, Roy Fielding's doctoral dissertation is hosted on [University of California, Irvine](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm).





