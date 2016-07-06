---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Solid Principles
date:   2016-07-06 10:18:00
tags: linux/unix
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---


The SOLID principals are five basic principles of object oriented programming and design.

* [Single](#composite) responsibility principle
* [Open](#open) closed principle
* [Liskov](#liskov) substitution principle
* [Interface](#interface) segregation principle
* [Dependency](#dependency) inversion principle

### Single Responsibility

a class should have only a single responsibility 
They are transparent; it’s easy to understand the code and it’s clear what will happen if it changes.

### Open/Closed Principle

code should be open for extension, but closed for modification

### Liskov Substitution

When you honor the contract, you are following the Liskov Substitution
Principle, which is named for its creator, Barbara Liskov, and supplies the “L”
in the SOLID design principles.
Her principle states:
Let q(x) be a property provable about objects x of type T. Then q(y)
should be true for objects y of type S where S is a subtype of T.
Mathematicians will instantly comprehend this statement; everyone else
should understand it to say that in order for a type system to be sane, sub-
types must be substitutable for their supertypes.
Following this principle creates applications where a subclass can be used
anywhere its superclass would do, and where objects that include modules
can be trusted to interchangeably play the module’s role.

### Interface Segregation

many client specific interfaces are better than one general purpose interface

### Dependency Inversion

one should depend upon Abstractions, do not depend upon concretions

