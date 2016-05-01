---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Finding Duck Types
date:   2016-05-01 15:18:00
tags: ruby
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

Duck typing is a design technique that relies on our ability to recognize the places where our application would benefit from across-class interfaces. Duck types are public interfaces that are not tied to any specific class. Its one that has an agreement about its public interface. If an object quacks like a duck and walks like a duck, then its class is immaterial, it’s a duck.

When using an object we should not be concerned about its class. Class is just one way for an object to acquire a public interface. It’s not what an object is that matters, it’s what it does. We are looking to create an object that trusts all others to be what it expects at any given moment, and any of those objects can be any kind of thing.

Its when we bring our objects behaviors to a more absract level and the `BodyShop` doesnt care about the engine or upolstery, it only cares that the object being passed has something body related that it can respond to. 

````ruby
class Car
  attr_reader :body, :engine, :upholstry, 
  def fix_with(type)
    type.repair(self)
  end
end

gto67.fix_with(BodyShop.new)
gto67.fix_with(GasMechanic.new)
gto67.fix_with(Upolstery.new)
````

You may already have ducks hidden somewhere in your codebase. Using some of the methods below is good indication that we may have duck types in our code.

* `case` statements that switch on class
* `kind_of?` and `is_a?`
* `responds_to?`

Having `case` statments that switch on class is a sign of a hidden duck. `kind_of?` will flat out ask if it is a specific class. `responds_to?` is better because it helps remove the hard coded class name, but we can still do a lot better.

````ruby
parts.each do |part|
  case part
  when Wheel
    part.polish_rim(wheels)
  when Window
    part.wipe_with_cleaner(winshield)
  when Body
    part.gently_wash(paneling)
  end
end

if part.kind_of?(Wheel)
  part.polish_rim(wheels)
elsif part.kind_of?(Window)
  ## ommiting the rest ##

if part.reponds_to?(:polish_rim)
  part.polish_rim(wheels)
elsif part.responds_to?(:wipe_with_cleaner)
  ## ommiting the rest ##
````

These types of methods to find out who the object is an instance of to figure out what they do, breaks the type of trust for objects to collaborate on an abstract level. Its the interface that matters, not the class of the object that implements it. Flexible applications are built on objects that operate on trust; it is our job to make your objects trustworthy. This type of code also introduces dependencies that make code difficult to change.

All of these `part` messages share something in common, on a higher abstract level, they clean something.

````ruby
class Car
  def clean
    parts.each do |part|
      part.wash(self)
    end
  end
end
````
106

