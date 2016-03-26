---
layout: post
cover: 'assets/images/crashing_waves.jpg'
title: A Brief Introductory into Design Patterns
date:   2016-03-23 10:18:00
tags: oop ruby
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

This post is a work in progress, check back later for updates!


###Template

The template design pattern is useful when the application is prone to change at a given interval. While behaviors are initially defined in a base class (our "ReadHTMLFile" class), they can be inherited into a class that allows for greater customization.

````
class ReadHTMLFile
  ### omitted for brevity ###
  def html_version
    puts "<!DOCTYPE html>"
  end

  def read
    html_version
    @file.each_line {|line| puts line}
  end
end

class ReadHTML401Trans < ReadFile
  def html_version
    puts '<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">'
  end
end

class ReadXHTML10Strict < ReadFile
  def html_version
    puts '<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">'
  end
end
````

Our base class will read a HTML5 without any problems. But if we plan on reading some older XHTML1.0 Strict or HTML4.01 Trans files, we would instantiate one of the subclasses, which would overwrite our current `#html_version` with the correct doctype. So the Template strategy starts with a base class, and then its subclass will make the changes needed to suit to the given condition.

One of the drawbacks of this design pattern, is that it relies heavily on inheritance. The subclasses will always depend on its baseclass, and will limit our flexibility.

###Strategy

The Strategey design pattern is based on composition and delegation rather than inheritance. It is where you pull the varying algorithm into separate object, where it will then pass it into the initializing subclass. The strategy gets passed into the context. 

````
class Accountant
  def work
    ### furiously presses numbers on keypad ###
  end
end

class Fireman
  def work
    ### saves kitten in tree ###
  end
end

class Person
  def initialize(type)
    @type = type
  end

  def preform_job
    @type.work
  end
end
````

Another way we can utilize the Strategy pattern is for the context to pass its `self` into a strategy method

````
class Fireman
  def work(context)
    puts "Fireman #{context.name} climbs the tree to save kitten."
    ### additional information reguarding Joe s life story ###
  end
end

class Person(strategy)
  def initialize
    @name = 'Joe'
    @job = strategy
  end

  def preform_job
    @job.work(self)
  end
end
````

However, this way has increased the coupling between classes and the context still needs a way to call the strategy, so there is still an ivar with the strategy.

The major benefit of strategy is the separation of concerns, varying elements have been taken out and put into thier own class.

###Observer

###Composite

###Iterator

###Command
