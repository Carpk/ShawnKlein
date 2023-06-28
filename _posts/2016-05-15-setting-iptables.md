---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Setting Iptables
date:   2016-05-15 10:18:00
tags: unix/linux
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

There are 5 tables independent tables. We can view a specific table's rules with the `-t` _table_ option, followed by the table's name. Using `sudo iptables -t nat -L` would list the rules for the nat table.

<table>
  <tr>
    <th>table</th>
    <th>description</th>
    <th>chains</th>
  </tr>
  <tr>
    <td>filter</td>
    <td>Default table</td>
    <td>INPUT, FORWARD, OUTPUT</td>
  </tr>
  <tr>
    <td>nat</td>
    <td>New connection packet</td>
    <td>PREROUTING, OUTPUT, POSTROUTING</td>
  </tr>
  <tr>
    <td>mangle</td>
    <td>Specialized packet alteration</td>
    <td>PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING</td>
  </tr>
  <tr>
    <td>raw</td>
    <td>Configuring exemptions</td>
    <td>PREROUTING, OUTPUT</td>
  </tr>
  <tr>
    <td>security</td>
    <td>Mandatory Access Control networking rules</td>
    <td>SECMARK, CONNSECMARK, INPUT, OUTPUT, FORWARD</td>
  </tr>
</table>

Each table has a list of chains, and may include some user defined chains. A chain is a list of rules which can match a set of packets. Each rule specifies what to do with a packet that matches. This is called a _target_, which may point to a user-defined chain in the same table.

<table>
  <tr>
    <th>chain</th>
    <th>description</th>
  </tr>
  <tr>
    <td>INPUT</td>
    <td>packets destined to local sockets</td>
  </tr>
  <tr>
    <td>OUTPUT</td>
    <td>locally generated packets</td>
  </tr>
  <tr>
    <td>FORWARD</td>
    <td>packets being routed</td>
  </tr>
  <tr>
    <td>PREROUTING</td>
    <td>altering  incoming packets before routing</td>
  </tr>
  <tr>
    <td>POSTROUTING</td>
    <td>altering packets as they are about to go out</td>
  </tr>
</table>


#### Commands

After either specifying a table, or using the default `filter` table, first 4 (of many) basic commands are:

<table>
  <tr>
    <th>option</th>
    <th>name</th>
    <th>description</th>
  </tr>
  <tr>
    <td>-A</td>
    <td>append</td>
    <td>appends to a chain</td>
  </tr>
  <tr>
    <td>-C</td>
    <td>check</td>
    <td>checks existence of rule in chain</td>
  </tr>
  <tr>
    <td>-D</td>
    <td>delete</td>
    <td>deleted rule from chain</td>
  </tr>
  <tr>
    <td>-L</td>
    <td>list</td>
    <td>list all rules, or of a chain if specified</td>
  </tr>
</table>

After one of our commands, we would typically specify a chain such as `-A INPUT`.

#### Options

<table>
  <tr>
    <th>option</th>
    <th>name</th>
    <th>description</th>
  </tr>
  <tr>
    <td>-m</td>
    <td>match</td>
    <td>Specifies extension module to test for a specific property.</td>
  </tr>
  <tr>
    <td>-p</td>
    <td>protocol</td>
    <td>follows with a protocol: `tcp`, `udp`, `udplite`, `icmp` or `all`</td>
  </tr>
  <tr>
    <td>-D</td>
    <td>delete</td>
    <td>delete rule from chain</td>
  </tr>
</table>

#### Targets

We can target an item from the list below, a user defined chain, or an extension.

<table>
  <tr>
    <th>target</th>
    <th>description</th>
  </tr>
  <tr>
    <td>ACCEPT</td>
    <td>allow packet through</td>
  </tr>
  <tr>
    <td>DROP</td>
    <td>deny the packet's access</td>
  </tr>
  <tr>
    <td>RETURN</td>
    <td>stop traversing this chain and resume at the next rule in the previous (calling) chain.</td>
  </tr>
</table>


#### Creating Rules

###### Allowing

Now with the basics out of the way, we will have a basic understanding of what is happening, when we try to append our first rule. We are going to allow estalished sessions to recieve traffic.

`sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT`

We are telling `iptables` to `--append` to the `INPUT` chain for incomming packets, with a `--match` from the `conntrack` extension where the connection state `--ctstate` is `ESTABLISHED` or `RELATED`, and for packets that meet this rule, to `--jump` to the target, `ACCEPT` in this case.

The `conntrack` module extension provides provides access to the connection tracking state. Passing in `--ctstate` allows us to match connection state, other states to match include: `INVALID`, `NEW`, `UNTRACKED`, `NONE`, `EXPECTED`. More info regarding this and other modules and can be found in `man iptables-extensions`. But in our case, `ESTABLISHED` matches a packet that is associated with a connection which has seen packets in both directions. `RELATED` will match a packet that is starting a new connection, but is associated with an existing connection.

Next, we will allow incoming traffic on port 22 for SSH. 

`sudo iptables -A INPUT -p tcp --dport ssh -j ACCEPT`

`iptables` will `--append` to the `INPUT` chain for incomming packets, the `--protocol` of `tcp` where destination port `--dport` matches `ssh`, these will `--jump` to the `ACCEPT` target. When the `--protocol` is used, it will load the module of the protocol type. So in the above command, iptables is loading the `tcp` module, and could be easily found in `man iptables-extensions` if we search for `--protocol tcp`, where we can see other commands that tcp will accept, such as `--sport`, `--tcp-flags`, `--syn`, and `--tcp-option`.

Command for allowing all web traffic

`sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT`

Another `--append` to the `INPUT` chain for incomming packets, the `--protocol` of `tcp` where destination port `--dport` matches `80`, these will `--jump` to the `ACCEPT` target.

###### Blocking

Once we have a rule to allow a packet, no more rules will effect it as long as those allowing rules come first. We can now write a statement to block all traffic, but ssh and web traffic on port 80 will be let through before it gets to this rule. 

`sudo iptables -A INPUT -j DROP`

This code is simple enough, `--append` to `INPUT` chain for all incoming packets, and `DROP` them. 

###### Saving

We will lose all configurations settings of our firewall upon restarting our server. In order to persist our settings, we will have to save using `iptables-save`. This will save to all tables, or we can specify using the _table_ option `iptables-save -t nat`.




