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
String address = "http://example.com";
String charset = "UTF-8";

URLConnection connection = new URL(address).openConnection();
connection.setRequestProperty("Accept-Charset", charset);
InputStream response = connection.getInputStream();
````

We use the `URLConnection` instance to `setRequestProperty()` on our `connection`. The URLConnection class allows us to set additional parameters on our object. Such as `setDefaultUseCaches(boolean)` and `setDoInput(boolean)`. If no additional parameters need to be set, then we could have immediately instantiated the `InputSteam` with `InputStream response = new URL(url).openStream();`.

To use `HttpURLConnection`, we still set up with `URL()` and  `openConnection()`, then use type casting to let our object to let our object be refrenced as a type of the `HttpURLConnection` class.

````java
String address = "http://example.com";
String charset = "UTF-8";
String userAgent = "Mozilla/5.0";

URL url = new URL(address);
HttpURLConnection connection = (HttpURLConnection) url.openConnection();

connection.setRequestMethod("GET");
connection.setRequestProperty("User-Agent", userAgent);

int responseCode = connection.getResponsecode();
````

The use of `HttpURLConnection` allows us more methods to use with our HTTP connection, such as `setRequestMethod("GET")`. Next, we will explore some addtional parameters to set when sending a post request.


````java
String address = "https://posttestserver.com/post.php";
String charset = "UTF-8";
String userAgent = "Mozilla/5.0";
String addressParameters = "sn=C02G8416DRJM&cn=&locale=&caller=&num=12345";

URL url = new URL(address);
HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();

connection.setRequestMethod("POST");
connection.setRequestProperty("User-Agent", userAgent);
connection.setRequestProperty("Accept-Language", "en-US, en;q=0.5");
connection.setDoOutput(true);

DataOutputStream wr = DataOutputStream(connection.getOutputStream());
wr.writeBytes(addressParameters);
wr.flush();
wr.close();
````

Setting up the connection is mostly the same as our previous examples, but we changed the request method to a post request. Then we called `setDoOutput()` and `getOutputStream()`, which area actually from the `URLConnection` class, and use `DataOutputStream` class to write the string to our post request. To test if this work, head over to [http://posttestserver.com/](http://posttestserver.com/) and check the logs!





