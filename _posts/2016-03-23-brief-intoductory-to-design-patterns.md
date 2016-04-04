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

We will be covering the following design patterns in this post:

* [Template](#template)
* [Strategy](#strategy)
* [Observer](#observer)
* [Composite](#composite)
* [Iterator](#iterator)
* [Command](#command)
* [Adapter](#adapter)
* [Proxy](#proxy)
* [Decorator](#decorator)
* [Singleton](#singleton)
* [Factory](#factory)
* [Builder](#builder)
* [Interpreter](#interpreter)


##<a name="template"></a>Template

The template design pattern is useful when the application is prone to change at a given interval. While behaviors are initially defined in a base class (our "ReadHTMLFile" class), they can be inherited into a class that allows for greater customization.

````ruby
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

One of the drawbacks of this design pattern, is that it relies heavily on inheritance. The subclasses will always depend on its base class, and will limit our flexibility.

##<a name="strategy"></a>Strategy

The Strategy design pattern is based on composition and delegation rather than inheritance. It is where you pull the varying algorithm into separate object, where it will then pass it into the initializing subclass. The strategy gets passed into the context. 

````ruby
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

````ruby
class Fireman
  def work(context)
    puts "Fireman #{context.name} climbs the tree to save kitten."
    ### additional information regarding Joe's life story ###
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

The major benefit of strategy is the separation of concerns, varying elements have been taken out and put into their own class.

##<a name="observer"></a>Observer

If we have potential that multiple objects are interested in the connection, they can register using the `#add_observers()` method. Otherwise, the below `@observers` array could be a single instantiation of an observing class that gets passed in upon the subjects initialization. The below example removes that implicit coupling as it is not dependant on whether none, one or many observer classes are in the array.

````ruby
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

There are two different techniques to notify the observer, the pull technique `observer.call(self)`, which just sends the subject. Then the push technique `observer.update(self,  :salary_changed, old_salary, new_salary)` or `observer.update_salary(self, old_salary, new_salary)` sends a great deal of additional information.


##<a name="composite"></a>Composite

Composite pattern is a combination of multiple elements coming together to make one overall element. This tree-like structure starts at the top with the component, the very bottom of the tree are leaf classes, and the classes that fill the between classes are composite classes.

````ruby
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


##<a name="iterator"></a>Iterator

Iterators pattern will provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

#####External

Is when the iterator is separate from the aggregate. The client drives the iteration, it will not call for the next element until it is ready for it. The external iterator will check if the array has an object available in the next field using `#has_next?`, then in the `#next_item` we pull the object out using the `@index` on the array, increment the `@index` by one, and use and implicit return with the `value` we initially set.

````ruby
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

````ruby
array.each {|e| **code block**}
````

Ruby iterators use the [Enumerable module](http://ruby-doc.org/core-2.3.0/Enumerable.html), more specifically, the `<=>` operator. The class you are iterating through needs some way for the iterator to compare values. If you are to create your own iterator, it would behoove us to include this mixin.

##<a name="command"></a>Command

The main idea of the Command pattern is to factor out the action code into its own object. It is the separation of concerns, from the code that does not change, to the code that does. to instantiate what the object is going to be executing. We are  

In our example below, we could have different Command classes. ShowCommand, HideCommand, SaveCommand, DeleteCommand, but the code that will not change with all these different commands is brought out into the UniveralButton class.

````ruby
class ShowCommand
  def description
    "show " + @ivars
  end
  def execute
    ## Something will be shown ##
  end
end

class UniveralButton
  def initialize(command)
    @command = command
    @logger = File.open('/')
  end
  def push_button
    @command.execute if @command
    @log.append @command.description
  end
end

button = UniversalButton.new(ShowCommand.new)
````

Using the Command pattern also helps to log executed commands. Our ShowCommand is a class, so it should have some state information to add to our description, then it can be logged to a file for future use. Perhaps you dont want to execute the same command twice? A safeguarded code block may check our log file to see if the code has already been ran. Or could be used as an undo, knowing which command was executed last could help determine what to do in order to 'undo' that last function. The [Madeleine](https://github.com/ghostganz/madeleine) is a great example of doing just that.

##<a name="adapter"></a>Adapter

An adapter is an object that crosses the chasm between the interface that you have and the interface that you need. Our below example has a `EmployeeManager` class, that would typically take an instanitated object of the `Employee` class. But we have a transfer from France, we would need a way for our french employee to interface with the employee manager. The `FrenchEmployeeConverter` will do just that, it will take our French employee and use its interface to coraspond with the changes it needs to make to be of use in the `EmployeeManager` class.

````ruby
class EmployeeManager
  def initialize
    @employee_list = []
  end
  ### Ommited different and fasinating ways to manage an employee ###
end
class Employee
  attr_reader :name, :position, :id
end
class FrenchEmployeeConverter
  def initialize(employé)
    @employé = employé
  end
  def name
    @employé.prénom
  end
  def position
    @employé.poste
  end
  def id
    @employé.ça
  end
end
````

Another easy way to do this, is to modify the class after its been instantiated, this will allow us to omit the adaptor class and use the original class as a regular `Employee` class.

````ruby
employee = FrenchEmployee.new

class << employee
  def name
    prénom
  end
  ### etc. etc. ###
end
````

This teqnique works well if the changes are simple, and we have deep knowledge of what our class is doing. However, if the changes are complex or we dont have a great understanding of what our class is doing, its safer to use `FrenchEmployeeConverter` class for our adaptor.

##<a name="proxy"></a>Proxy

Proxy classes have the real object, the subject, hidden inside themselves. Giving us another seperation of concerns. In our example, our employee data is hidden behind a EmployeeDataProxy that verifies the current user is authorized to view the sensitive data regarding a particular employee.

````ruby
class EmployeeData
  attr_reader :id, :name, :ssn
end
class EmployeeDataProxy
  def initialize(employee = Employee.new)
    @employee = employee
  end
  def name
    @employee.name
  end
  def ssn
    @employee.ssn if auth_user
  end
  def auth_user
    ### verifies permissions to view sensitive docs ###
  end
end
````

We can also use proxy classes for lazy loading. The employee subject does not get loaded until a method is called that needs to have the subject loaded. 

````ruby
class EmployeeDataProxy
  def initialize(&creation_block)
    @creation_block = creation_block
  end
  def name
    e = subject
    e.name
  end
  def subject
    @subject ||= @creation_block.call
  end
end
````

Instead of having the `#subject` method instantiate a Employee class directly, we will pass in a proc object and use that code block when needed `EmployeeDataProxy.new(Employee.new)`. This lowers coupling and allows other ways of creating an employee subject, `EmployeeDataProxy.new(Employee.find(#))`.

##<a name="decorator"></a>Decorator



##<a name="singleton"></a>Singleton

##<a name="factory"></a>Factory

##<a name="builder"></a>Builder

##<a name="interpreter"></a>Interpreter

