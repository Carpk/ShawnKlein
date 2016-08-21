---
layout: post
cover: 'assets/images/sloping_mountains.jpg'
title: The OSI Model
date:   2016-06-19 10:18:00
tags: general devops
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

The Open Systems Interconnection (OSI) model is a way to help us conceptualize how data gets transfered from one point to another. This post describes the recommended 7 layered model, where 1 is the lowest layer in this model.

<table style="width:100%">
  <tr>
    <td>#</td>
    <td>Layer</td>
    <td>PDU</td> 
    <td>Functions</td>
    <td>Examples</td>
  </tr>
  <tr>
    <td>7</td>
    <td>Application</td>
    <td>Data</td> 
    <td>resource sharing, remote file access, directory services</td>
    <td>HTTP, LADP, SSH, FTP, SMTP, DNS</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Presentation</td>
    <td>Data</td> 
    <td>data conversion, compression, encryption</td>
    <td>SSL, Kerberos, JPEG, GIF, MIME</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Session</td>
    <td>Data</td> 
    <td>manages session for multiple data segments, continuous exchange of data between nodes</td>
    <td>NetBIOS, NFS, PAP, SCP, SQL, ZIP</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Transport</td>
    <td>Segment, Datagram</td> 
    <td>reliable transmission of data segments, acknowledgements and multiplexing</td>
    <td>TCP, UDP</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Network</td>
    <td>Packet</td> 
    <td>addressing, routing</td>
    <td>IPv4, IPv6, ICMP</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Data Link</td>
    <td>Frame</td> 
    <td>point to point data transmission</td>
    <td>Mac addresses, PPP</td>
  </tr>
  <tr>
    <td>1</td>
    <td>Physical</td>
    <td>Bit</td>
    <td>transmits raw bit streams</td>
    <td>Ethernet, RJ-45, physical connection</td>
  </tr>
</table>

Each layer is built on the one below it. So we will decompile it from the bottom up.
<ol>
  <li>Our physical layer, there can be no data transmission without some means of a physical connection. This layer includes wireless cards, and old network hubs.</li>
  <li>The Data Link layer supports connetions from one node (your PC) to the very next connected node (your router). A great example of theses step by step node connections is to run a `traceroute` to a URL. Each connection is a router hop that uses the Data Link layer for its connection.


````
K55N:~/Projects $ traceroute example.com
traceroute to example.com (93.184.216.34), 30 hops max, 60 byte packets
1  homeportal (192.168.1.254)  1.589 ms  3.571 ms  4.364 ms
2  107-207-56-3.lgtspeed.ciril.sbglobal.net (107.208.54.2)  358.589 ms  360.831 ms  362.335 ms
3  71.145.65.208 (71.145.65.208)  358.151 ms  358.471 ms  362.285 ms
4  12.83.43.53 (12.83.43.53)  363.783 ms  363.237 ms 12.83.43.49 (12.83.43.49)  364.004 ms
5  gar13.cgcil.ip.att.net (12.122.132.121)  364.762 ms  365.137 ms  365.140 ms
````
  </li>
  <li>Networking layer provides the end to end connection. It would be the connection from our PC to the web server to view this page. The above Data Link layer doesn't know how to get to this point, instead it says "but I know someone who does" and keeps transfering the next router. 
  
<table style="width:100%">
  <tr>
    <td>port</td>
    <td>from</td>
    <td>to</td> 
  </tr>
  <tr>
    <td>3</td>
    <td>198.0.0.1</td>
    <td>198.0.0.50</td> 
  </tr>
  <tr>
    <td>6</td>
    <td>198.0.0.51</td>
    <td>198.0.0.100</td> 
  </tr>
</table>

  This is an over simplified version of a routing table, if the router recieves traffic destined to `198.0.0.78`, it checks the table, which states to send that traffic out on port 6. The reciever on that end continues the same process until it narrows down to the node.
  </li>
  <li>Now that we can get information from one node to another, the next set of layers are more concerned with that actual moving of our data. Transport layer provides reliable, and multiplexing ports.
  
<table>
  <tr>
    <td>Protocol</td>
    <td>Applications</td>
    <td>to</td> 
  </tr>
  <tr>
    <td>TCP</td>
    <td>HTTP, FTP, Telnet</td>
    <td>error checking, connection oriented, ordered, uses ACK, heavyweight</td> 
  </tr>
  <tr>
    <td>UDP</td>
    <td>VOIP, live streaming video</td>
    <td>no error checking, connection-less, unordered, no ACK, lightweight</td> 
  </tr>
</table>

  TCP will hold a packet if it recieves it out of order. Once it recieves the missing packet, it will process the held packet. This is to ensure our data is interpeted in the correct way. UDP will drop a packet if it recieves it out of order.
  
  ACK is a three way handshake to ensure data has been properly recieved. UDP has no use for this as it is dependant on real time. We wouldnt want to watch a clip of something that happend 30 seconds ago during a stream of live video. The same would go for a phone call.

  Speacial note for multiplexing: when you send a HTTP request for a webpage, the request will be on port 80, which how the server knows to translate it as an HTTP request. But the returning port will be an arbitrary number with the requestee's IP address. This arbitrary number is how our local machine is able to multiplex and keep track of which application sends the request.
  </li>
  <li>The Session layer sets up, coordinates, and terminates conversations, exchanges, and dialogues between the applications at each end. It deals with session and connection coordination. SQL is in this layer as our application needs to set up and cordinate with our database server.</li>
  <li>The presentation layer works to transform data into the form that the application layer can accept. This layer formats and encrypts data to be sent across a network, providing freedom from compatibility problems</li>
  <li>Everything at this layer is application-specific. This layer provides application services for file transfers, e-mail, and other network software services. Telnet and FTP are applications that exist entirely in the application level.</li>
</ol>













