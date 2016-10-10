---
layout: post
cover: 'assets/images/autumn_stairs.jpg'
title: Java URLConnection Class
date:   2016-09-10 10:18:00
tags: coding java
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

One of the most useful classes in Java is the `URLConnection` class. Its the base class of `HttpURLConnection` and `HttpsURLConnection`, which we will also cover. 

All three of these classes are abstract classes, this means we are not able in instantiate them. Instead we use the `URL` class and send that to our abstract class.


````java
String url = "http://example.com";

URLConnection connection = new URL(url).openConnection();
````



















