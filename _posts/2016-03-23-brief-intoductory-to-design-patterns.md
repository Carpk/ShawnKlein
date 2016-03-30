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

* [Template](#template)

* [Strategy](#strategy)

* [Observer](#observer)

* [Composite](#composite)

* [Iterator](#iterator)

* [Command](#command)

* Adapter

* Proxy

* Decorator

* Singleton

* Factory

###<a name="template"></a>Template

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

###<a name="strategy"></a>Strategy

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

Another way we can utilize the Strategy pattern is for the context to pass its `self` into a strategy method. The example below also has an example if you were to pass the strategy in as a proc. We would bundle up our `#work` method into a proc object and pass it in when initializing our Person class. Which could be used when the strategy interface is a very simple design, such as our example below.

````
class Fireman
  def work(context)
    puts "Fireman #{context.name} climbs the tree to save kitten."
    ### additional information reguarding Joe s life story ###
  end
end

class Person
  def initialize(strategy) # or &strategy
    @name = 'Joe'
    @job = strategy
  end

  def preform_job
    @job.work(self)
      # or
    # @job.call(self)
  end
end
````

However, this way has increased the coupling between classes and the context still needs a way to call the strategy, so there is still an ivar with the strategy.

The major benefit of strategy is the separation of concerns, varying elements have been taken out and put into thier own class.

###<a name="observer"></a>Observer

If we have potential that multiple objects are interested in the connection, they can register using the `#add_observers()` method. Otherwise, the below `@observers` array could be a single instantiation of an observing class that gets passed in upon the subjects initialization. The below example removes that implicit coupling as it is not dependant on whether none, one or many observer classes are in the array.

````
class Logger
  def update(obj)
    @log_file << "#{obj} is attempting to make a connection"
  end
end

class Subject
  def initialize
    @observers = [Logger.new]
  end

  def add_observer(obs)
    @observers << obs
  end

  def report_to_observers
    @observers.each {|obs| obs.update(self)}
  end
end

class Connection
  include Subject
  def initialize
    super
  end

  def connect
    ### attempts to connect ###
    report_to_observers
  end
end
````

Ruby comes with its own observer module that can be found in the [standard library](http://ruby-doc.org/stdlib-2.0.0/libdoc/observer/rdoc/Observable.html). 

One variation is to use code blocks when passing in to the `#add_observable(&observer)` method.

There are two differant techniques to notify the observer, the pull technique `observer.call(self)`, which just sends the subject. Then the push technique `observer.update(self,  :salary_changed, old_salary, new_salary)` or `observer.update_salary(self, old_salary, new_salary)` sends a great deal of additional information.


###<a name="composite"></a>Composite

Composite pattern is a combination of multiple elements coming together to make one overall element. This tree-like structure starts at the top with the component, the very bottom of the tree are leaf classes, and the classes that fill in the inbetween are composite classes.

````
class Company
  def initialize
    @job = Department.new
  end
end

class Department
  def initialize
    @location = String.new
    @sub_tasks = []
  end
  def num_of_tasks
    total = 0
    @sub_tasks.each {|task| total += task.num_of_tasks}
    total
  end
end

class Position
  def num_of_tasks
    total = 1
  end
end

class Employee
end
````


###<a name="iterator"></a>Iterator

Iterators pattern will provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

#####External

Is when the iterator is separate from the aggregate. The client drives the iteration, it will not call for the next element until it is ready for it. The external iterator will check if the array has an object available in the next field using `#has_next?`, then in the `#next_item` we pull the object out using the `@index` on the array, increment the `@index` by one, and use and implicit return with the `value` we initially set.

````
class ExternalIterator
  def initialize(array)
    @array = array
    @index = 0
  end
  def has_next?
    @index < @array.length
  end
  def item
    @array[@index]
  end
  def next_item
    value = @array[@index]
    @index += 1
    value
  end
end

i = ExternalIterator.new([3,4,5])
while i.has_next?
  puts i.next_item
end
````
#####Internal

With internal iterators, the aggregate will push the code block to accept item after item.

````
array.each {|e| **do stuff**}
````

###<a name="command"></a>Command

###<a name="adapter">Adapter

###<a name="proxy">Proxy

###<a name="decorator">Decorator

###Singleton

###Factory

###Builder

###Interpreter

