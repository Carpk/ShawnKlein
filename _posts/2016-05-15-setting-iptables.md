---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Setting Iptables
date:   2016-05-15 10:18:00
tags: linux/unix
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---


The `iptables` firewall allows all traffic by default. If we just set up our server, we are not going to have any rules as of yet. But to get an idea of what a table may look like, we use `sudo iptables -L` to list our rules for our default `filter` table. We _list_ the rules by using the `-L` option, we can also specify a chain after our list option, `sudo iptables -L INPUT` would only show the rules for the INPUT chain.

````
~$ sudo iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
````

####Tables

There are 5 tables independant tables. We can view a specific table's rules with the `-t` _table_ option, followed by the table's name. Using `sudo iptables -t nat -L` would list the rules for the nat table.

<table>
  <tr>
    <th>table</th>
    <th>description</th>
    <th>chains</th>
  </tr>
  <tr>
    <td>`filter`</td>
    <td>Default table</td>
    <td>INPUT, FORWARD, OUTPUT</td>
  </tr>
  <tr>
    <td>`nat`</td>
    <td>New connection packet</td>
    <td>PREROUTING, OUTPUT, POSTROUTING</td>
  </tr>
  <tr>
    <td>`mangle`</td>
    <td>Specialized packet alteration</td>
    <td>PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING</td>
  </tr>
  <tr>
    <td>`raw`</td>
    <td>Configuring exemptions</td>
    <td>PREROUTING, OUTPUT</td>
  </tr>
  <tr>
    <td>`security`</td>
    <td>Mandatory Access Control networking rules</td>
    <td>SECMARK, CONNSECMARK, INPUT, OUTPUT, FORWARD</td>
  </tr>
</table>

We see the names for the 5 tables, and thier distinct chains. 

#### Commands

After either specifying a table, or using the default `filter` table, first 3 (of many) basic commands are:

<table>
  <tr>
    <th>option</th>
    <th>name</th>
    <th>description</th>
  </tr>
  <tr>
    <td>`-A`</td>
    <td>--append</td>
    <td>appends to a chain</td>
  </tr>
  <tr>
    <td>`-C`</td>
    <td>`--check`</td>
    <td>checks existence of rule in chain</td>
  </tr>
  <tr>
    <td>`-D`</td>
    <td>`--delete`</td>
    <td>delete rule from chain</td>
  </tr>
</table>

After a command, we would also specify a chain `-A INPUT`

#### Options

<table>
  <tr>
    <th>option</th>
    <th>name</th>
    <th>description</th>
  </tr>
  <tr>
    <td>`-m`</td>
    <td>match</td>
    <td>Specifies extension module to test for a specific property.</td>
  </tr>
  <tr>
    <td>`-p`</td>
    <td>protocol</td>
    <td>follows with `protocol`, `tcp`, `udp`, `udplite`, `icmp` or `all`</td>
  </tr>
  <tr>
    <td>`-D`</td>
    <td>`--delete`</td>
    <td>delete rule from chain</td>
  </tr>
</table>























