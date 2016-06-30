---
layout: post
cover: 'assets/images/rocky_river.jpg'
title: HTTP Standard
date:   2016-06-25 9:48:00
tags: general
subclass: 'post tag-test tag-content'
categories: 'Casper'
navigation: True
logo: 'assets/images/logo.png'
---

The HTTP protocol is a request/response protocol. A client sends a request to the server in the form of a request method, URI< and protocol version, followed by a MIME-like message containing request modifiers, client information, and possible body content over a connection with a server. 

![image of HTTP request](/assets/images/http_request.jpg)

The server responds with a status line that includes the message's protocol version and a response code for success or error, followed by a MIME-like message containing server information, entity metainformation, and possible entity-body content.

![image of HTTP request](/assets/images/http_response.jpg)

HTTP is also used as a generic protocol for communication between
   user agents and proxies/gateways to other Internet systems, including
   those supported by the SMTP [16], NNTP [13], FTP [18], Gopher [2],
   and WAIS [10] protocols.
   
### Versions

The first version was documentated in 1991.

#### HTTP/0.9

Was a simple protocol for raw data transfer.


#### HTTP/1.0

Improved the protocol by allowing messages to be in the format of MIME-like messages, containing metainformation about the data transferred and modifiers on the request/response semantics.
A separate connection to the same server is made for each resource request. 

* MIME (Multipurpose Internet Mail Extensions) were designed for SMTP/email service, however, it became of an importance when using HTTP.  This enables the browser to display or output files that are not in HTML format.

#### HTTP/1.1

Can reuse a connection multiple times to download resources. Such as images, scripts, and stylesheets, after the page has been delivered.

#### HTTP/2.0


#### Request methods


<ul>
  <li>Get</li>
  <li>HEAD</li>
  <li>POST</li>
  <li>PUT</li>
  <li>DELETE</li>
  <li>TRACE</li>
  <li>OPTIONS</li>
  <li>CONNECT</li>
  <li>PATCH</li>
</ul>

#### TCP/IP

Hypertext Transfer Protocol (HTTP) is an application level protocol. We covered applicaiton level protocols in an [OSI model blog post](/the-osi-model),






