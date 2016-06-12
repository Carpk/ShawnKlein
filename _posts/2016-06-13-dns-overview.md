---
layout: post
cover: 'assets/images/railroad_fog.jpg'
title: DNS Overview
date:   2016-04-26 20:24:00
tags: general 
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---


A Domain Name System (DNS) is typically provided by your ISP. It is used to look up a hostname (ShawnKlein.net) and convert it to a IP address (127.0.0.1) so we can find our site on the World Wide Web. The DNS converts more than just hostnames, the table below will help us make sense of some additional records that we may be familiar with.


<table>
  <tr>
    <th>Name</th>
    <th>Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>A</td>
    <td>IPv4 address</td>
    <td>32-bit IP address, used to map hostnames</td>
  </tr>
  <tr>
    <td>AAAA</td>
    <td>IPv6 address</td>
    <td>128-bit IP address, used to map hostnames</td>
  </tr>
  <tr>
    <td>CCA</td>
    <td>Canonical name</td>
    <td>Alias of one name to another, DNS lookup will continue with new name</td>
  </tr>
  <tr>
    <td>CERT</td>
    <td>Certificate</td>
    <td>Stores PGP</td>
  </tr>
  <tr>
    <td>CNAME</td>
    <td>Canonical name</td>
    <td>Alias of one name to another, DNS lookup will continue with new name</td>
  </tr>
  <tr>
    <td>MX</td>
    <td>Mail exchange</td>
    <td>prefix line with its line number match</td>
  </tr>
  <tr>
    <td>SOA</td>
    <td>Start of Authority</td>
    <td>Alias of one name to another, DNS lookup will continue with new name</td>
  </tr>
  <tr>
    <td>URI</td>
    <td>Uniform Resource Identifier</td>
    <td>maps hostnames</td>
  </tr>
</table>

This is only a small sample of DNS record types, a full list can be found on ![wikipedia.org](https://en.wikipedia.org/wiki/List_of_DNS_record_types)

It is helpful to note that a URL is a type of URI. A URL being `http://ShawnKlein.net/awesome_page`, so a URI can refer to the same, in addition to URNs such as `urn:isbn:0-486-27557-4`.


![DNS query diagram](/images/dns_queries.gif)


The root name server begins the proccess by reading the domain name from right to left. Directing us to another server that matches our TLD (.net, .com, .uk, .gov).

Our com. nameserver can then resolve the hostname, which would be the `example` in `http://www.example.com`

Then the example.com nameserver can respond to the final portion, being a `www` request, it will return the appropriate IP address. 








