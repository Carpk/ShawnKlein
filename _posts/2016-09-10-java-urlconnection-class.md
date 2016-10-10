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

All three of these classes are abstract classes, this means we are not able in instantiate them. Instead we start by initializing the `URL` class with the address we are looking to query, and by calling `openConnection()` will return a `URLConnection` instance. 

````java
String url = "http://example.com";
String charset = "UTF-8";

URLConnection connection = new URL(url).openConnection();
connection.setRequestProperty("Accept-Charset", charset);
InputStream response = connection.getInputStream();
````

We use the `URLConnection` instance to `setRequestProperty()` on our `connection`. The URLConnection class allows us to set additional parameters on our object. Such as `setDefaultUseCaches(boolean)` and `setDoInput(boolean)`. If no additional parameters need to be set, then we could have immediately instantiated the `InputSteam` with `InputStream response = new URL(url).openStream();`.

















