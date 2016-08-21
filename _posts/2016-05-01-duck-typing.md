---
layout: post
cover: 'assets/images/rocky_river.jpg'
title: Duck Typing
date:   2016-05-01 15:18:00
tags: ruby coding oop
subclass: 'post tag-test tag-content'
categories: 'Casper'
navigation: True
logo: 'assets/images/logo.png'
---

Duck typing is an object oriented design technique that lets us determine an object by using its properties and its methods. We are not concerned with the class type of a duck object, it's the public interface that will dictate how we may interact with the object. Our goal is to utilize a duck's public interface with other similar ducks and allow them to share their interfaces in an abstract manner. We are trying to recognize the places in our code that our application would benefit from having similar interfaces.

If we have a common behavior among our classes, they can share a public interface.

````ruby
class BodyShop
  def repair
    ### preforms body work ###
  end
end
class GasMechanic
  def repair
    ### fixes gas engines ####
  end
end
class UpholsteryService
  def repair
    #### services interior ###
  end
end
````

If need be, that interface can except `self` for which ever attributes our duck object may need. In our example below, the `BodyShop` object doesn't care about the car's engine or upholstery, it only cares that the object being passed to itself has something body related that it can respond to.

````ruby
class Car
  attr_reader :body, :engine, :upholstery, 
  def fix_at(repair_shop)
    repair_shop.repair(self)
  end
end

gto68.fix_at(BodyShop.new)
gto68.fix_at(GasMechanic.new)
gto68.fix_at(UpholsteryService.new)
````
The `Car` class doesn't know what type of object was passed in for `#fix_at()`'s argument, nor should it care. If the object has the behavior of `#repair()` and does it like a `GasMechanic` and does other `GasMachanic` things, then it must be a `GasMechanic`. This is where we apply "If an object quacks like a duck and walks like a duck, then its class is immaterial, it’s a duck".

We are looking to create an object that trusts all others to be what it expects at any given moment, and any of those objects can be any kind of thing. What helps direct us, is the fact that the `#fix_at()` method serves a single purpose. Its argument arrives wishing to accomplish a single goal, it's class is less important than what the `#fix_at()` needs to do. Its goal is to repair something, and the design allows the passing object to do so.

We may already have ducks hidden somewhere in our codebase. A good indication may be the usage of some of the methods below.

* `case` statements that switch on class
* `kind_of?` and `is_a?`
* `responds_to?`

Having `case` statements that switch on class is a sign of a hidden duck. `kind_of?` will also directly ask if it is of a specific class. `responds_to?` is better because it helps remove the hard coded class name, but we can still do a lot better.

````ruby
parts.each do |part|
  case part
  when Wheel
    part.polish_rim(wheels)
  when Window
    part.wipe_with_cleaner(windshield)
  when Body
    part.gently_wash(paneling)
  end
end

if part.kind_of?(Wheel)
  part.polish_rim(wheels)
elsif part.kind_of?(Window)
  ## omitting the rest ##

if part.responds_to?(:polish_rim)
  part.polish_rim(wheels)
elsif part.responds_to?(:wipe_with_cleaner)
  ## omitting the rest ##
````

In the first section of our example, we depend on a concrete class, which makes it dangerous to extend. These types of methods that try to find out who the object is an instance of, in order to figure out what they do, breaks the type of trust for objects to collaborate on an abstract level. Its the interface that matters, not the class of the object that implements it. Flexible applications are built on objects that operate on trust; it is our job to make your objects trustworthy. This type of code also introduces dependencies that make code difficult to change.

All of these `part` objects share something in common, on a higher abstract level, they clean something.

````ruby
class Car
  def clean
    parts.each do |part|
      part.wash(self)
    end
  end
end
````

This makes it easy to extend, if we were ever to add an addtional class to our `parts` array, the only behavior we would need from it is for a `wash` public interface.

