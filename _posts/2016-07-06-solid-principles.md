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

We are going to take a look at each principle, and break it down with some simple Ruby examples.

### Single Responsibility

> A class should have one, and only one, reason to change.

A class should have only a single responsibility. When the requirements change, that change will be shown through a change in responsibility amongst the classes. If a class assumes more than one responsibility, then there will be more than one reason for it to change, and responsibilities are an axis of change. Responsibilities become coupled, changes to one, may impair or inhibit the class's ability to meet the others. This leads to fragile designs that break in unexpected ways when changed.

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

Now we have two smaller classes that handle each specific task. Our `BankAccount` class will process any bank related activities, and our `Person` class will handle any people type behaviors. The classes are also transparent, it’s easy to understand the code and it’s clear what will happen if it changes.

While the original documentation spoke in terms of classes for the SRP, it can also be applied to methods. A good rule of thumb is if you need to use the words "and" or "or" to describe what your method does, then it is doing too much.

### Open/Closed Principle

> code should be open for extension, but closed for modification

Let's break this up and take a closer look at each portion of our open/closed principle:

<ul>
  <li>Code should be open for extension. This means that the behavior of the module can be extended. That we can make the module behave in new and different ways as the requirements of the application change, or to meet the needs of new applications.</li>
  <li>Code should be closed for modification. The source code of such a module is inviolate. No one is allowed to make source code changes to it.</li>
</ul>

Code that follows the open/closed principle is easy to extend functionality without having to modifying the existing code. Below, we have a file parser that requires us to make modifications when changing how the file will parser with certain file formats.

````ruby
class BuildTaco
  def initialize(order)
    @order = order
  end
  def create
    case @order.shell
      when :hard_taco
        prep_with_hard_shell
      when :soft_taco
        prep_with_soft_shell
    end
    # finish making taco
  end
  def prep_with_hard_shell
    # put shell in taco box
  end
  def prep_with_soft_shell
    # wrap shell in paper
  end
end
````

We would have to modify our `BuildTaco` when having to make changes to the way it preps with a shell. It would also require many changes if/when we decide to add a new type of shell, such as wrapped in lettuce. This violates the open/closed principle, there is no way to extend our class to include our lettuce wrap, and we would have to modify the code to make any changes.


````ruby
class BuildTaco
  def initialize(order, shell)
    @order = order
    @shell = shell
  end
  def create
    @shell.prep
    # finish making taco
  end
end
class HardShell
  def prep
    # put shell in taco box
  end
end
class SoftShell
  def prep
    # wrap shell in paper
  end
end
````

Now we have the ability to add new types of wraps without changing any code. It is simple to create a `LettuceWrap` class and pass it in to our `BuildTaco` with the rest of the order and the duck type will do the rest.

### Liskov Substitution

>   Derived classes must be substitutable for their base classes.

This principle states that you should be able to replace any instances of a parent class with one of its children, without unexpected or incorrect behaviors. Any children instances should be able to preform the same tasks as its parent class.

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

The principle states that a client should not be forced to depend on methods that it does not use. When designing a class, we should not have a "fat" public interface full of methods other classes wont use.

````ruby
class Salad
  def eat_with_fork
  end
end
class Soup
  def eat_with_fork
  end
end
````
We certainly cannot eat soup with a fork, so this is a poor interface implementation. We are attempting to force clients of this interface to depend on these methods that they do not want to use.

### Dependency Inversion

> Depend on abstractions, not on concretions.

This principle suggests we abstract out any concrete implementations. We do not want our classes to have any hard coded dependencies, instead, they should be passed in when instantiating or set with a method. This allows us to use duck typing when implementing our classes.

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
    #process for combining an ingredients_array
  end
end
class CakeBatter
  def combine
    #process for combining an ingredients_array
  end
end
````
Instead of hard coding in a `@pizza_batter = PizzaBatter.new` or `@cake_batter = CakeBatter.new` somewhere in the `MixBatter` class, we will pass these objects in when instantiating our class. The `MixBatter` class does not care whether its mixing pizza batter, cake batter, or an egg batter, it only cares that the injected class is able to respond to `#combine`.
 

