---
layout: post
cover: 'assets/images/rocky_river.jpg'
title: HTTP Standard
date:   2016-06-27 9:48:00
tags: general
subclass: 'post tag-test tag-content'
categories: 'Casper'
navigation: True
logo: 'assets/images/logo.png'
---

HTTP is a request/response protocol that is used for transmitting hypermedia documents such as HTML. HTTP connections start by a client sending a request to the server in the form of a [request method](#methods), [URI](#uri), and [protocol version](#versions), followed by a MIME-like message containing request modifiers, client information, and possible body content over a connection with a server. 

![image of HTTP request](/assets/images/http_request.jpg)

The server responds with a status line that includes the message's [protocol version](#versions) and a [response code](#codes) for success or error, followed by a MIME-like message containing server information, entity metainformation, and possible entity-body content.

![image of HTTP request](/assets/images/http_response.jpg)

HTTP is also used as a generic protocol for communication between user agents and proxies/gateways to other Internet systems, including those supported by the SMTP, FTP, Gopher, and WAIS protocols.

* MIME (Multipurpose Internet Mail Extensions) were designed for SMTP/email service, however, it became important when using HTTP to enable browsers to display or output files that are not in HTML format.

####<a name="methods"></a>Request methods

HTTP defines methods to indicate the desired action to be preformed on the identified resource. These methods are as follows:

<table style="width:100%">
  <tr>
    <th>Method</th>
    <th>Function</th> 
  </tr>
  <tr>
    <td>GET</td>
    <td>Requests a representation of the specified resource.</td> 
  </tr>
  <tr>
    <td>HEAD</td>
    <td>Used when retrieving meta-information from response headers. Similar to GET, but server responds with no body.</td> 
  </tr>
  <tr>
    <td>POST</td>
    <td>requests the server accepts the entity enclosed in the request as a new subordinate</td> 
  </tr>
  <tr>
    <td>PUT</td>
    <td>Requests enclosed entity be stored under supplied URI</td> 
  </tr>
  <tr>
    <td>DELETE</td>
    <td>Deletes the specified resource</td> 
  </tr>
  <tr>
    <td>TRACE</td>
    <td>Echoes the recieved request, to see if any changes are made by intermediate servers</td> 
  </tr>
  <tr>
    <td>OPTIONS</td>
    <td>Returns supported HTTP methods for specified URL. Used to check functionality</td> 
  </tr>
  <tr>
    <td>CONNECT</td>
    <td>Converts connection to TCP/IP tunnel, Used for encrypted communication through an unencrypted proxy.</td> 
  </tr>
  <tr>
    <td>PATCH</td>
    <td>Applies partial modifications to a resource</td> 
  </tr>
</table>


####<a name="uri"></a>URI

A URL is a type of URI. HTTP will use a URL such as `http://www.example.com/` or `ftp://example.com`, to send requests. It includes the prefixes access mechanism, being `http` and `ftp` in our examples. 

A URN is also a type of URI, although it is much simplier as it only refers to the name of a resource `example.com` `urn:isbn:0451450523`. As you can see, there is no application layer protocols associated in these examples.

URIs are used 

####<a name="versions"></a>Protocol Versions

Intro to protocol versions
The first version was documentated in 1991.

##### HTTP/0.9

Was a simple protocol for raw data transfer. connection is closed after a single request/response pair


##### HTTP/1.0

Improved the protocol by allowing messages to be in the format of MIME-like messages, containing metainformation about the data transferred and modifiers on the request/response semantics.
A separate connection to the same server is made for each resource request. connection is closed after a single request/response pair

##### HTTP/1.1

Can reuse a connection multiple times to download resources. Such as images, scripts, and stylesheets, after the page has been delivered. keep-alive-mechanism was introduced, allowing a connection to be reused for more than one request. This greatly reduced latency, as the connection does not need to re-negotiate the 3-way handshake. 

##### HTTP/2.0

Goal of HTTP/2.0 is to improve performance by reducing latency and the number of TCP requests. Its utilizes multiplexing, header compression, server push, and resource prioritization.

####<a name="codes"></a>Response Codes

404

#### TCP/IP

Hypertext Transfer Protocol (HTTP) is an application level protocol. We covered application level protocols in the [OSI model blog post](/the-osi-model). An HTTP client initiates a session by using a TCP connection to port 80 of the server. This session  starts with a 3 way handshake.

<ol>
  <li>The client will sent a `syn` packet, requesting the server to establish a session.</li>
  <li>Server will respond to `syn` with `syn-ack`, stating it wishes to synchronize with the requesting client, and ackowleges its initial request.</li>
  <li>Finally, the client will validate the server's synchronize request by sending a `ack` to acknowledge its synchronize request. 
</ol>

![image of 3 way handshake for TCP connection](/assets/images/3-way-handshake.jpg)


###### Notes

With the keep-alive mechanism being introduced in HTTP/1.1, allowing more than one request to be made during a connection. This _persistent connection_ 

