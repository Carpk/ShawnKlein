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

Let's take a look at each principle. 

### Single Responsibility

> A class should have one, and only one, reason to change.

The class should have only a single responsibility. They shoujld be transparent, it’s easy to understand the code and it’s clear what will happen if it changes. When the requirements change, that change will be manifest through a change in responsibility amongst the classes. If a class assumes more than one responsibility, then there will be more than one reason for it to change. Responsibilities become coupled, changes to one, may impair or inhibit the class's ability to meet the others. This leads to fragile designs that break in unexpected ways when changed.

````ruby
class Person
  
  def collect_check
    check = talk_to_hr_about_pay   
    add_money(check)
  end
  
  def add_money(deposit)
    @checking += deposit * 0.95
    @savings += deposit * 0.05
  end
end
````

Any change on how we add money to our bank account, would require a change to this class. We could introduce new rules or strategies that would cause our `#add_money()` method to change.

````ruby
class Person

  def initialize
    @bank_account = BankAccount.new
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

Now we have 2 smaller classes that handle each specific task. Our `BankAccount` class will proccess any bank related activities, and our `Person` class will handle any people type behaviors.

### Open/Closed Principle

> code should be open for extension, but closed for modification

Lets take a look at what means to be open and closed:

<ul>
  <li>A module will be said to be open if it is still available for extension. This means that the behavior of the module can be extended. That we can make the module behave in new and different ways as the requirements of the application change, or to meet the needs of new applications.</li>
  <li>A module will be said to be closed if it is available for use by other modules.The source code of such a module is inviolate. No one is allowed to make source code changes to it.</li>
</ul>

that is, such an entity can allow its behaviour to be extended without modifying its source code.

### Liskov Substitution

>   Derived classes must be substitutable for their base classes.

When you honor the contract, you are following the Liskov Substitution Principle, which is named for its creator, Barbara Liskov, and supplies the “L” in the SOLID design principles. Her principle states:
Let q(x) be a property provable about objects x of type T. Then q(y)
should be true for objects y of type S where S is a subtype of T.
Mathematicians will instantly comprehend this statement; everyone else
should understand it to say that in order for a type system to be sane, sub-
types must be substitutable for their supertypes.
Following this principle creates applications where a subclass can be used anywhere its superclass would do, and where objects that include modules can be trusted to interchangeably play the module’s role.

### Interface Segregation

> Make fine grained interfaces that are client specific.

many client specific interfaces are better than one general purpose interface

### Dependency Inversion

> Depend on abstractions, not on concretions.

one should depend upon Abstractions, do not depend upon concretions

