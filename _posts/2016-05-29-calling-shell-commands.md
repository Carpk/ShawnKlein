---
layout: post
cover: 'assets/images/rocky_river.jpg'
title: Calling Shell Commands
date:   2016-05-29 17:09:00
tags: ruby coding
subclass: 'post tag-test tag-content'
categories: 'Casper'
navigation: True
logo: 'assets/images/logo.png'
---

Occasionally we need to interact with the OS or run a shell command, Ruby gives us a few ways to do this. All commands allow for interpolation.

#### System

The `system()` call will execute the command in a subshell. Returns `true` if command was found, `nil` if not.

````bash
irb(main):0> `ls`
bin  README.md	src
=> true
````

#### Backtics 

This returns the stdout of the shell command. It does not return stderr, must append `2>&1` for error messages.

````bash
irb(main):0> `ls`
=> "bin\nREADME.md\nsrc\n"
````

#### Exec 

Command takes over current process. Does not return anything as it is in control of another process.

````bash
irb(main):0> `ls`
bin  README.md	src
~$
````

#### Built-in Syntax %x( )

Same as using backtics, but with using our choice of delimiters. Returns stdout of sent command.

````bash
irb(main):0> %x(ls)
=> "bin\nREADME.md\nsrc\n"
````




