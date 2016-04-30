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

86





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


