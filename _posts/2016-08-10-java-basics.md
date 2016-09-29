---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Java Basics
date:   2016-08-10 10:18:00
tags: coding java
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

This post attempts to cover some of the basics in Java. Its meant for someone with some basic programming knowledge, but still new to Java. We will talk about some commonly used keywords, classes, and cover some light troubleshooting skills.

To create a runnable Java object, we need to compile our source code using `javac` and our `.java` file. After compiled, we have Java _bytecode_, that can be ran on numerous devices. 

````
~$ javac test.java
~$ java Test
````

Your `CLASSPATH` env variable has to be set to our working folder to get `java Test` to run properly. Otherwise you have to specify it from our root directory `java -cp /home/user/Projects/ Test`. Our `-cp` is short for `-classpath`, meaning class search path of directories and zip/jar files.

###### Keywords

We use these keywords when defining and class and its methods. 

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
  <tr>
    <td>abstract</td>
    <td>In front of a `class` keyword, prevents this class to be directly instantiated. In front of a method signature, allows the implementation of this method to be deferred to an inheriting class.</td>
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

This section is a high level overview of some broad concepts in Java. Concepts such as Packages, Thread Pools and Abstract classes.

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

We should consider using interface when:

* Expect that unrelated classes would implement you interface.
* Specify the behavior of a particular data type, but not concerned about who implements its behavior.
* Want to take advantage of multiple inheritance.

###### Abstract

Abstract classes cannot be instantiated, but they can be subclassed. They may contain a mix of methods declared with or without an implementation. Its is different from `Interface` as you can declare fields that are not `static` and `final`, and define `public`, `protected`, and `private` methods.

````java
abstract class GraphicObject {
  int x, y;
  void moveTo(int newX, int newY) {...}
  abstract void draw();
  abstract void resize();
}
class Circle extends GraphicObject {
  void draw() {...}
  void resize() {...}
}
class Rectangle extends GraphicObject {
  void draw() {...}
  void resize() {...}
}
````

We should consider using abstract classes when:

* We want to share code among several closely related clases.
* Expect the classes that extend your abstract class to have many common methods or fields, or require access modifiers other than public.
* Want to declare non-static or non-final fields.

###### Package

A package is a namespace that organizes a set of related classes and interfaces. Conceptually you can think of packages as being similar to different folders on your computer. You might keep HTML pages in one folder, images in another, and scripts or applications in yet another. Because software written in the Java programming language can be composed of hundreds or thousands of individual classes, it makes sense to keep things organized by placing related classes and interfaces into packages.

The Java platform provides an enormous class library (a set of packages) suitable for use in your own applications. Its packages represent the tasks most commonly associated with general-purpose programming. For example, a String object contains state and behavior for character strings; a File object allows a programmer to easily create, delete, inspect, compare, or modify a file on the filesystem; a Socket object allows for the creation and use of network sockets; various GUI objects control buttons and checkboxes and anything else related to graphical user interfaces. There are literally thousands of classes to choose from. This allows us to focus on the design of our particular application, rather than the infrastructure required to make it work.

###### Inheritance

We use inheritance when we want to create a new class and there is already a class that includes some of the code that you want, you can derive your new class from the existing class. In doing this, you can reuse the fields and methods of the existing class without having to write and debug them yourself.

A subclass inherits all the members (fields, methods, and nested classes) from its superclass. Constructors are not members, so they are not inherited by subclasses, but the constructor of the superclass can be invoked from the subclass.

###### Thread pools

A thread pool manages the pool of worker threads, it contains a queue that keeps tasks waiting to get executed. A thread pool manages the collection of Runnable threads and worker threads execute Runnable from the queue. java.util.concurrent.Executors provide implementation of java.util.concurrent.Executor interface to create the thread pool in java.

### Classes

A class can be defined as a blueprint that describes the behavior and state that the object of its type support. We compiled a small list of notable classes that deserve some reconigtion.

#### Conext

Conext is a `public interface`. As the name suggests, it's the context of current state of the application/object. It lets newly created objects understand what has been going on. Typically you call it to get information regarding another part of your program (activity and package/application).


<table style="width:100%">
  <tr>
    <th>Type</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>void</td>
    <td>bind(String name, Object obj)</td> 
    <td>Binds name to object.</td>
  </tr>
  <tr>
    <td>void</td>
    <td>close()</td> 
    <td>Closes this context.</td>
  </tr>
  <tr>
    <td>Hashtable</td>
    <td>getEnvironment()</td> 
    <td>Retrieves the environment in effect for this context.</td>
  </tr>
  <tr>
    <td>Object</td>
    <td>lookup(String name)</td> 
    <td>Retrieves the named object.</td>
  </tr>
</table>




#### System

Thread is a `public class`. A thread is a thread of execution in a program. The JVM allows an application to have multiple threads running concurrently.

<table style="width:100%">
  <tr>
    <th>Type</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>void</td>
    <td>start()</td> 
    <td>Causes this thread to begin execution; the Java Virtual Machine calls the run method of this thread.</td>
  </tr>
  <tr>
    <td>static void</td>
    <td>yield()</td> 
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>getEnvironment()</td> 
    <td>Retrieves the environment in effect for this context.</td>
  </tr>
  <tr>
    <td></td>
    <td>lookup(String name)</td> 
    <td>Retrieves the named object.</td>
  </tr>
</table>

#### Object

The `Object` class is the root of the class hierarchy.

<table style="width:100%">
  <tr>
    <th>Type</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>protected Object</td>
    <td>clone()</td> 
    <td>Creates and returns a vopy of this object.</td>
  </tr>
  <tr>
    <td>protectd void</td>
    <td>finalize()</td> 
    <td></td>
  </tr>
  <tr>
    <td>Class</td>
    <td>getClass()</td> 
    <td>Retrieves the environment in effect for this context.</td>
  </tr>
  <tr>
    <td></td>
    <td>notify()</td> 
    <td>Wakes up a single thread that is waiting on this object's monitor.</td>
  </tr>
  <tr>
    <td>String</td>
    <td>toString()</td> 
    <td>Returns a string representation of the object.</td>
  </tr>
  <tr>
    <td></td>
    <td>wait()</td> 
    <td>Causes the current thread to wait until another thread invokes the notify() or notifyAll() method.</td>
  </tr>
</table>




#### HttpURLConnection

The `HttpURLConnection` is a `public abstract class`. Each instance is used to make a single request, but the underlying network connection to the HTTP server may be transparently shared by other instances.

<table style="width:100%">
  <tr>
    <th>Type</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>String</td>
    <td>getRequestMethod()</td> 
    <td>Get the response method.</td>
  </tr>
  <tr>
    <td>String</td>
    <td>getResponseMessage()</td> 
    <td>Gets the HTTP response message, if any, returned along with the response code from a server.</td>
  </tr>
  <tr>
    <td>void</td>
    <td>setRequestMethod(String url)</td> 
    <td>Set the method for the URL request, one of: GET POST HEAD OPTIONS PUT DELETE TRACE are legal, subject to protocol restrictions.</td>
  </tr>
  <tr>
    <td>abstract boolean</td>
    <td>usingProxy()</td> 
    <td>Indicates if the connection is going through a proxy.</td>
  </tr>
</table>




#### URLConnection

The `URLConnection` is a `public abstract class`. URLConnection is constructed using `URLConnection(URL url)`. Known subclasses are `HttpURLConnection` and `JarURLConnection`. 

<table style="width:100%">
  <tr>
    <th>Type</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>abstract void</td>
    <td>connect()</td> 
    <td>Opens a communications link to the resource referenced by this URL, if such connection has not already been established.</td>
  </tr>
  <tr>
    <td>InputStream</td>
    <td>getInputStream()</td> 
    <td>Returns an input stream that reads from this open connection.</td>
  </tr>
  <tr>
    <td>OutputStream</td>
    <td>getOutputStream()</td> 
    <td>Returns and output stream that writes to this connection.</td>
  </tr>
  <tr>
    <td>void</td>
    <td>setDoOutput(boolean dooutput)</td> 
    <td>Sets the value of the doOutput field for this URLConnection.</td>
  </tr>
  <tr>
    <td>void</td>
    <td>setRequestProperty(String key, String value)</td> 
    <td>Sets the general request property.</td>
  </tr>
</table>

````java
private final String USER_AGENT = "Mozilla/5.0"
String charset = "UTF-8";
String url = "http://www.example.com/"

URLConnection connection = new URL(url).openConnection();
connection.setRequestProperty("Accept-Charset", charset);

InputStream response = connection.getInputStream();
````



#### OutputStream

The `OutputStream` is a `public abstract class`. part of the java.io package. This abstract class is the superclass of all classes representing an output stream of bytes.

<table style="width:100%">
  <tr>
    <th>Type</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>void</td>
    <td>close()</td> 
    <td>Closes this output stream and releases any system resources associated with this stream</td>
  </tr>
  <tr>
    <td>void</td>
    <td>flush()</td> 
    <td>Flushes this output stream and forces any buffered output bytes to be written out.</td>
  </tr>
  <tr>
    <td>void</td>
    <td>write(byte[] b)</td> 
    <td>Writes b.length bytes from the specified byte array to this output stream. Also take other forms of arguments.</td>
  </tr>
</table>




#### Reader

The `Reader` is a `public abstract class`, found in the `java.io` package. Abstract class for reading character streams. The only methods that a subclass must implement are read(char[], int, int) and close(). Most subclasses, however, will override some of the methods defined here in order to provide higher efficiency, additional functionality, or both.

<table style="width:100%">
  <tr>
    <th>Type</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>abstract void</td>
    <td>close()</td> 
    <td>Closes the stream and releases any system resources associated with it.</td>
  </tr>
  <tr>
    <td>int</td>
    <td>read()</td> 
    <td>Reads a single charater. Argument also take (char[]), (char[], int, int) and (CharBuffer)</td>
  </tr>
  <tr>
    <td>boolean</td>
    <td>ready()</td> 
    <td>Tells whether this stream is ready to be read.</td>
  </tr>
</table>




#### Thread

The `Thread` is a `public class` found in the `java.lang` package. A thread is a thread of execution in a program. The Java Virtual Machine allows an application to have multiple threads of execution running concurrently.


<table style="width:100%">
  <tr>
    <th>Type</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>static Thread</td>
    <td>currentThread()</td> 
    <td>Returns a reference to the currently executing thread object.</td>
  </tr>
  <tr>
    <td>void</td>
    <td>start()</td> 
    <td>Causes this thread to begin execution; the Java Virtual Machine calls the run method of this thread.</td>
  </tr>
  <tr>
    <td>String</td>
    <td>getName()</td> 
    <td>Returns this thread'd name.</td>
  </tr>
  <tr>
    <td>Thread.state</td>
    <td>getState()</td> 
    <td>Returns the state of this thread.</td>
  </tr>
  <tr>
    <td>void</td>
    <td>join()</td> 
    <td>Waits for this thread to die.</td>
  </tr>
  <tr>
    <td>static void</td>
    <td>sleep(long millis)</td> 
    <td>Causes the currently executing thread to sleep for the specified number of milliseconds.</td>
  </tr>
  <tr>
    <td>static void</td>
    <td>yield()</td> 
    <td>A hint to the scheduler that the current thread is willing to yield its current use of a processor.</td>
  </tr>
</table>



