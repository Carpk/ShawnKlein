---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Java Basics
date:   2016-05-11 10:18:00
tags: coding java
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

To creae a runnable Java object, we need to compile our source code. After compiled, we have Java _bytecode_, that can be ran on numerous devices.

````
~$ javac Test.java
~$ java Test
````

###### Keywords

These keywords will define addition behaviors when defining a class or method.

<table>
  <tr>
    <th>Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>final</td>
    <td>defines an entity once that cannot be changed nor derived from</td>
  </tr>
  <tr>
    <td>static</td>
    <td>class maintains one copy of field, regardless of how many instances exist</td>
  </tr>
  <tr>
    <td>void</td>
    <td>methods does not return a value</td>
  </tr>
  <tr>
    <td>interface</td>
    <td>Used to declare a special type of class that only contains abstract methods,</td>
  </tr>
  <tr>
    <td>extends</td>
    <td>specify the superclass</td>
  </tr>
  <tr>
    <td>implements</td>
    <td>specify one or more interfaces that are implemented by the current class</td>
  </tr>
</table>


Access Modifiers are also keywords, but are in thier own category as you typically always use one.

<table>
  <tr>
    <th>Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>private</td>
    <td>only accessible within the same class as it is declared</td>
  </tr>
  <tr>
    <td>no modifier</td>
    <td>accessible within classes in the same package</td>
  </tr>
  <tr>
    <td>protected</td>
    <td>accessible within all classes in the same package and within subclasses in other packages</td>
  </tr>
  <tr>
    <td>public</td>
    <td>accessible to all classes</td>
  </tr>
</table>


### Concepts

I'm going to skip classes, objects, and inheritance, and touch on a couple things that are unique to java.

###### Interface

Implementing an interface allows a class to become more formal about the behavior it promises to provide. Interfaces form a contract between the class and the outside world, and this contract is enforced at build time by the compiler. If your class claims to implement an interface, all methods defined by that interface must appear in its source code before the class will successfully compile.

We also want to note that this is the proper way to `implement` an interface. We declare it in the class declaration.

````java
interface Bicycle {
    void speedUp(int increment);
    void applyBrakes(int decrement);
}

class ACMEBicycle implements Bicycle {

    int speed = 0;

   // The compiler will now require that methods
   // changeCadence, changeGear, speedUp, and applyBrakes
   // all be implemented. Compilation will fail if those
   // methods are missing from this class.

    void speedUp(int increment) {
         speed = speed + increment;   
    }
    void applyBrakes(int decrement) {
         speed = speed - decrement;
    }
    void printStates() {
         System.out.println("speed: " + speed );
    }
}

````

###### Package

A package is a namespace that organizes a set of related classes and interfaces. Conceptually you can think of packages as being similar to different folders on your computer. You might keep HTML pages in one folder, images in another, and scripts or applications in yet another. Because software written in the Java programming language can be composed of hundreds or thousands of individual classes, it makes sense to keep things organized by placing related classes and interfaces into packages.

The Java platform provides an enormous class library (a set of packages) suitable for use in your own applications. This library is known as the "Application Programming Interface", or "API" for short. Its packages represent the tasks most commonly associated with general-purpose programming. For example, a String object contains state and behavior for character strings; a File object allows a programmer to easily create, delete, inspect, compare, or modify a file on the filesystem; a Socket object allows for the creation and use of network sockets; various GUI objects control buttons and checkboxes and anything else related to graphical user interfaces. There are literally thousands of classes to choose from. This allows you, the programmer, to focus on the design of your particular application, rather than the infrastructure required to make it work.

The Java Platform API Specification contains the complete listing for all packages, interfaces, classes, fields, and methods supplied by the Java SE platform. Load the page in your browser and bookmark it. As a programmer, it will become your single most important piece of reference documentation.


### Classes

We compiled a small list of notable classes that deserve some reconigtion.

###### Conext

Conext is a public interface. As the name suggests, it's the context of current state of the application/object. It lets newly-created objects understand what has been going on. Typically you call it to get information regarding another part of your program (activity and package/application).


<table style="width:100%">
  <tr>
    <th>Type</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>void</td>
    <td>close()</td> 
    <td>Closes this context.</td>
  </tr>
</table>

















