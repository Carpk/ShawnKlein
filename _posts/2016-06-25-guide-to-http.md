---
layout: post
cover: 'assets/images/rocky_river.jpg'
title: Guide to HTTP
date:   2016-06-27 9:48:00
tags: general devops
subclass: 'post tag-test tag-content'
categories: 'Casper'
navigation: True
logo: 'assets/images/logo.png'
---

HTTP is a request/response protocol that is used for transmitting hypermedia documents such as HTML. HTTP connections start by a client sending a request to the server in the form of a [request method](#methods), [URI](#uri), and [protocol version](#versions), followed by a [MIME](#mime)-like message containing request modifiers, client information, and possible body content over a connection with a server. 

The request method, URI and protocol version make up the __request line__. Followed by __request headers__ defining the parameters of the HTTP transaction. Both of these are what make up the __request message header__. Then lastly a __blank line__ and the optional __request message body__.

![image of HTTP request](/assets/images/http_request.jpg)

The server responds with a __status line__ that includes the message's [protocol version](#versions) and a [response code](#codes) for success or error, followed by by a [MIME](#mime)-like message containing server information, entity meta-information, which is our __response headers__ and possible a entity-body content being __response message body__.

![image of HTTP request](/assets/images/http_response.jpg)

HTTP is also used as a generic protocol for communication between user agents(browsers, web crawlers) and proxies/gateways to other Internet systems, including those supported by the SMTP, FTP, Gopher, and WAIS protocols. In this way, HTTP allows basic hypermedia access to resources available from diverse applications.

#### TCP

Hypertext Transfer Protocol (HTTP) is an application level protocol. We covered application level protocols in the [OSI model blog post](/the-osi-model). HTTP was designed to use TCP of the transport layer, but can be used with other protocols such as UDP. 

An HTTP client initiates by starting a TCP session to port 80 of the server. The server's TCP stack uses Transmission Control Block (TCB) for distinct connections. Now we begin with a 3 way handshake.

<ol>
  <li>The client will sent a `syn` packet, requesting the server to establish a session.</li>
  <li>Server will respond to `syn` with `syn-ack`, stating it wishes to synchronize with the requesting client, and acknowledges its initial request.</li>
  <li>Finally, the client will respond with `ack` to acknowledge the server's synchronize request. 
</ol>

![image of 3 way handshake for TCP connection](/assets/images/3-way-handshake.jpg)

TCP will then commence with data transmission, and will close the session upon completion.

#### Sessions

HTTP is a stateless protocol, that is dependent on a stateful TCP protocol, which relies on IP that is also stateless. Stateless protocols treat requests an independent transactions unrelated to any previous requests. HTTP follows the RESTful architecture, while TCP maintains state in the form of window size for data transmission. With HTTP being stateless, a web client can hold information in cookies or session IDs, such as authentication credentials to keep a session but still be stateless.

#### Encryption

HTTPS is the protocol for secure communication over the network. Its encrypted by the Transport Layer Security (SSL) or Secure Socket Layer (SSL), which is also found on the application layer of the OSI model.

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
    <td>Echoes the received request, to see if any changes are made by intermediate servers</td> 
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

A URL is a type of URI. HTTP will use a URL such as `http://www.example.com/` or `ftp://example.com`, to send requests. It includes a prefixed *access mechanism*, being `http` and `ftp` in our examples. 

A URN is also a type of URI, although it is much simpler as it only refers to the name of a resource `example.com` or `urn:isbn:0451450523`. As you can see, there is no application layer protocols associated in these examples.

Now that we have an overview of different types of URIs, we can now say that URIs are a string of characters that are used to identify a resource.

####<a name="versions"></a>Protocol Versions

HTTP went through different versions starting from 1991, when HTTP/0.9 was first documented.

##### HTTP/0.9

Simple protocol for raw data transfer. Connection is closed after a single request/response pair

##### HTTP/1.0

Improved the protocol by allowing messages to be in the format of MIME-like messages, contain meta-information about the data transferred and modifiers on the request/response semantics. Connection is still closed after a single request/response pair

##### HTTP/1.1

Can reuse a connection multiple times to download resources. Such as images, scripts, and stylesheets, after the page has been delivered. Keep-alive-mechanism was introduced, allowing a connection to be reused for more than one request. This greatly reduced latency, as the connection does not need to re-negotiate the 3-way handshake. 

##### HTTP/2.0

Goal of HTTP/2.0 is to improve performance by reducing latency and the number of TCP requests. Its utilizes multiplexing for multiple TCP connections, header compression reduces sending the same headers repeatedly, server push handles resource dependencies, and resource prioritization judges what the user will need first.

####<a name="mime"></a>MIME

MIME (Multipurpose Internet Mail Extensions) was designed for SMTP/email service, however, it became important when using HTTP to enable browsers to display or output files that are not in HTML format.

####<a name="codes"></a>Response Codes

The status line of an HTTP response includes a numeric status code and a textual reason phrase. Below is a small sample set, but servers can even have custom codes and proprietary codes from vendors.

<table style="width:100%">
  <tr>
    <th>Code</th>
    <th>Status</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>100</td>
    <td>Continue</td>
    <td>For large body requests, server has received the header and is ready for request body</td>
  </tr>
  <tr>
    <td>200</td>
    <td>OK</td>
    <td>Standard response for HTTP requests</td>
  </tr>
  <tr>
    <td>201</td>
    <td>Created</td>
    <td>Request has been fulfilled</td> 
  </tr>
  <tr>
    <td>202</td>
    <td>Accepted</td>
    <td>Request has been accepted for processing, but processing has not been completed</td> 
  </tr>
  <tr>
    <td>301</td>
    <td>Moved Permanently</td>
    <td>This and all future requests should be directed to the given URI</td> 
  </tr>
  <tr>
    <td>400</td>
    <td>Bad Request</td>
    <td>Server cannot process the request due to error</td> 
  </tr>
  <tr>
    <td>401</td>
    <td>Unauthorized</td>
    <td>Authentication has failed</td> 
  </tr>
  <tr>
    <td>403</td>
    <td>Forbidden</td>
    <td>User does not have proper permission for request</td> 
  </tr>
  <tr>
    <td>404</td>
    <td>Not Found</td>
    <td>Returns supported HTTP methods for specified URL. Used to check functionality</td> 
  </tr>
  <tr>
    <td>500</td>
    <td>Internal Server Error</td>
    <td>Generic error, no specific information is available</td> 
  </tr>
  <tr>
    <td>502</td>
    <td>Bad Gateway</td>
    <td>Server was acting as proxy/gateway and received invalid response from upstream server</td>
  </tr>
</table>



