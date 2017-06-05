---
layout: post
cover: 'assets/images/crashing_waves.jpg'
title: OpenVPN from the Command Line
date:   2017-06-05 10:18:00
tags: unix/linux devops
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---   

OpenVPN is a popular VPN service. Many companies that provide a VPN service use OpenVPN as their underlying client 

#### Basics


The easiest way to get started is with a config file in your current directory. The file could be in either `.ovpn` or `.conf` format.

````bash
~$ sudo openvpn chicago.ovpn

````

If you are recieving errors such as:

````bash
Options error: --ca fails with 'ca.rsa.2048.crt': No such file or directory
Options error: --crl-verify fails with 'crl.rsa.2048.pem': No such file or directory
````
It may be nessecary to have the `.pem` and `.crt` files accompany your config file.












