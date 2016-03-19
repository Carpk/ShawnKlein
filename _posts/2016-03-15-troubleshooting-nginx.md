---
layout: post
cover: 'assets/images/autumn_stairs.jpg'
title: Troubleshooting nginx install
date:   2016-03-15 10:18:00
tags: troubleshooting
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

Recently after installing nginx on a Ubuntu Server and completed setting up configurations along with the creation of a basic index.html page, I was unable to connect to the URL or even ping the ip address. The connection kept giving me a "request timed out" error.

I ssh'd to the server and ran a series of codes to check that nginx was running, this was my first step to make sure nginx was up and running:

`ps aux | grep nginx`

Lets break this up to see what is going on, 

`ps`

will show a a snapshot of running processes on the server, which may not be much, but with the options
`aux` we get alot more data.

`a` lifts the 'only yourself' restriction

`u` displays in user oriented format

`x` lifts the 'only with tty' restriction

Then we pipe that returning data into a `grep` command which will search the output for 'nginx'. I will not go into grep here, as it is something that deserves its own page on how to use it. But safe to say it will filter every line out that doesnt say 'nginix' on it.

The returning values should look something like this:

```
root      1010  0.0  0.2 140300  2512 ?        Ss   22:58   0:00 nginx: master process /usr/sbin/nginx
www-data  1011  0.0  0.3 140624  3276 ?        S    22:58   0:00 nginx: worker process
www-data  1012  0.0  0.3 140624  3276 ?        S    22:58   0:00 nginx: worker process
www-data  1013  0.0  0.3 140624  3276 ?        S    22:58   0:00 nginx: worker process
www-data  1014  0.0  0.3 140624  3060 ?        S    22:58   0:00 nginx: worker process
ubuntu    1390  0.0  0.0  10460   936 pts/0    S+   23:13   0:00 grep --color=auto nginx
```
If it does not, then you need start the nginx services using `sudo service nginx start`. But in our case, we are running and on to the next step. 

While still ssh'd to the server, run `telnet localhost 80` to attempt to the server. Being we made a connection, we are certain nginx is up, running, and doing its job. While still having a telnet connection, we can send an http request by typing `GET /index.html`. And we recieved the index page back.

We know nginx is running, and will reply with the proper values with an GET index request, the issues must be between the browser and the server. Either an DNS issue, or in our case, we checked the security poilicy settings and found AWS only allowed TCP port 22 to be open for our initial SSH sessions. After allowing incoming TCP port 80 for incoming http requests, our server is successfully showing our index page.
