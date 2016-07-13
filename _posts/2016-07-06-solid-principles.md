---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Solid Principles
date:   2016-07-06 10:18:00
tags: general ruby
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---


The SOLID principals are five basic principles of object oriented programming and design. These principles help engineers write maintainable code. 

* [Single](#composite) responsibility principle
* [Open](#open) closed principle
* [Liskov](#liskov) substitution principle
* [Interface](#interface) segregation principle
* [Dependency](#dependency) inversion principle

Let's take a look at each principle. 

### Single Responsibility

> A class should have one, and only one, reason to change.

A class should have only a single responsibility. When the requirements change, that change will be shown through a change in responsibility amongst the classes. If a class assumes more than one responsibility, then there will be more than one reason for it to change. And responsibilities are an axis of change. Responsibilities become coupled, changes to one, may impair or inhibit the class's ability to meet the others. This leads to fragile designs that break in unexpected ways when changed.

Our code below is very simplistic example, its meant to highlight that our `Person` class is performing more than one responsibility. It is in charge of keeping track of money, and performing a job.

````ruby
class Person
  def preform_job
    @position.work
  end
  def add_money(deposit)
    @checking += deposit * 0.95
    @savings += deposit * 0.05
  end
end
````

Our `Person` class has two reasons to change. First, the way our class will `#preform_job` could change, and any change on how we add money to our bank account would also require a change to this class. We could introduce new rules or strategies that would cause our `#add_money()` method to change. Or perhaps, our person will be delegating the workload onto a group of subordinates.

````ruby
class Person
  def preform_job
    @position.work
  end
  def deposit_money(amount)
    @bank_account.deposit(amount)
  end
end

class BankAccount
  def deposit(amount)
    @checking += deposit * 0.95
    @savings += deposit * 0.05
  end
end
````

Now we have two smaller classes that handle each specific task. Our `BankAccount` class will proccess any bank related activities, and our `Person` class will handle any people type behaviors. The classes are also transparent, it’s easy to understand the code and it’s clear what will happen if it changes.

### Open/Closed Principle

> code should be open for extension, but closed for modification

Let's take a closer look at what means to be open for extension and closed for modification:

<ul>
  <li>Code should be open for extension. This means that the behavior of the module can be extended. That we can make the module behave in new and different ways as the requirements of the application change, or to meet the needs of new applications.</li>
  <li>Code should be closed for modification. The source code of such a module is inviolate. No one is allowed to make source code changes to it.</li>
</ul>

Code that follows the open/closed principle is easy to extend functionality without having to modifiying the existing code. Below, we have a file parser that requires us to make modifications when changing how the file will parser with certain file formats.

````ruby
class FileParser
  def initialize(file)
    @file = file
  end
  def parse
    case @file.file_format
      when :xml
        parse_xml
      when :csv
        parse_csv
    end
    @file.timestamp = Time.now
    @file.save!
  end
  def parse_xml
    # parse xml
  end
  def parse_csv
    # parse csv
  end
end
````

We would have to modify our `FileParser` when having to make changes to the way it parses xml, csv, or different type of files. This violates the open/closed principle.  


````ruby
class FileParser
  def initialize(file, parser)
    @file = file
    @parser = parser
  end
  def parse(file)
    @parser.parse(file)
    @file.timestamp = Time.now
    @file.save!
  end
end
class XmlParser
  def parse(file)
    # parse xml
  end
end
class CsvParser
  def parse(file)
    # parse csv
  end
end
````

Now we have the ability to add new parsers without changing any code. It is simple to create a `YamlParser` class and pass it in to our `FileParser` with the file to be parsed and the code will do the rest.

### Liskov Substitution

>   Derived classes must be substitutable for their base classes.

This principle states that you should be a able to replace any instances of a parent class with one of its children, without unexpected or incorrect behaviors. Any children instances should be able to preform the same tasks as its parent class.

````ruby
class Rectangle
  def set_height(height)
    @height = height
  end
  def set_width(width)
    @width = width
  end
end

class Square < Rectangle
  def set_height(height)
    super(height)
    @width = height
  end
  def set_width(width)
    super(width)
    @height = width
  end
end
````

The instance of `Square` class will not behave the same way as an instance of `Rectangle`. Calling `#set_height` also changes our width. So our `Square` child instance cannot replace its `Rectangle` parent, thus breaking the Liskov Substitution principle.

### Interface Segregation

> Make fine grained interfaces that are client specific.

Ruby's dynamic typing makes a lot Interface Segregation go away on its own, but using `#is_a?` and `#responds_to?` are a form of 








### Dependency Inversion

> Depend on abstractions, not on concretions.

This principle suggests we abstract out any concrete implementations. We do not want our classes to have any hard coded dependencies, instead, they should be passed in when instantiating or setter method. This allows us to use duck typing when implementing out classes. We did this in the open/close principle above. 

````ruby
class FileParser
  def initialize(file, parser)
    @file = file
    @parser = parser
  end
  def parse(file)
    @parser.parse(file)
    @file.timestamp = Time.now
    @file.save!
  end
end
class XmlParser
  def parse(file)
    # parse xml
  end
end
class CsvParser
  def parse(file)
    # parse csv
  end
end
````


````ruby
class MixBatter
  def initialize(batter_type)
    @batter = batter_type
  end
  def mix_it
    @batter.combine
  end
end
class PizzaBatter
  def combine
    #process for combining an ingredents_array
  end
end
class CakeBatter
  def combine
    #process for combining an ingredents_array
  end
end
````
Instead of hard coding in a `XmlParser.new` or `CsvParser.new` somewhere in the `FileParser`, we will pass these objects in when instantiating our file parser. The file parser does not care whether we want to parse an XML, JSON, or CSV file, it only cares that the `@parser` object will respond to `#parse`.  

