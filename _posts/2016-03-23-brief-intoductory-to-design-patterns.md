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

This post is a brief overview of software design patterns from the Design Patterns in Ruby book. The book is a Ruby version of the original Gang of Four book- Design Patterns. We will be covering the following patterns in this post:

* [Template](#template) changes at a particular step
* [Strategy](#strategy) different algorithms to accomplish task
* [Observer](#observer) classes watching another class for changes
* [Composite](#composite) collection of objects that act as one
* [Iterator](#iterator) stores objects but allows access
* [Command](#command) wraps instructions in object
* [Adapter](#adapter) fixes interface mismatch
* [Proxy](#proxy) connects to networked object
* [Decorator](#decorator) adds additional behaviors to object
* [Singleton](#singleton) single instance everyone can use
* [Factory](#factory) subclass produces objects
* [Builder](#builder) builds complex objects
* [Interpreter](#interpreter) customize interface to solve problem


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

The Strategy design pattern is based on composition and delegation rather than inheritance. It is where we pull the varying algorithm into a separate object, then pass it into the initializing subclass. The strategy class is what gets passed into the context. 

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
  def initialize(job)
    @job = job
  end

  def preform_job
    @job.work
  end
end
````

Another way we can utilize the Strategy pattern is for the context class to pass its `self` into a strategy method. The example below also has commented out code for passing the strategy in as a proc. We would bundle up the workings of our `#work` method into a proc object and pass it in when initializing our Person class. Which ideally used when the strategy interface is a very simple design, such as our example below.

````ruby
class Fireman
  def work(context)
    puts "Fireman #{context.name} climbs the tree to save kitten."
    ### additional information regarding Joe's life story ###
  end
end

class Person
  def initialize(strategy) # &strategy
    @name = 'Joe'
    @job = strategy
  end

  def preform_job
    @job.work(self)
    # @job.call(self)
  end
end
````

However, this increases the coupling between classes. And the context still needs a way to call the strategy, forcing us to still use an ivar for the strategy.

The overall major benefit of the strategy pattern is the separation of concerns, varying elements have been taken out and put into their own class.

##<a name="observer"></a>Observer

If we have the potential that multiple other objects are interested in a particular class and its actions, they can register using the `#add_observers()` method. Otherwise, the below `@observers` array could be a single instantiation of an observing class that gets passed in upon the subjects initialization. The below example removes that implicit coupling as it is not dependant on whether none, one or many observer classes are in the array.

````ruby
class Logger
  ### additional logging information omitted ###
  def update(obj)
    @log_file << "#{obj} is attempting to make a connection"
  end
end
class Connection
  def initialize
    @observers = [Logger.new]
  end
  def add_observer(obs)
    @observers << obs
  end
  def report_to_observers
    @observers.each {|observer| observer.update(self)}
  end
  def connect
    ### attempts to connect ###
    report_to_observers
  end
end
````

Brevity is key in our examples, but it would be best practice if we broke out the observer details into a module and included it in our `Connection` class.

Our `@observer` array could also store procs, changing the `#add_observable(&observer)` and `observer.call(self)` methods would accomplish this. Our examples use the pull technique, `#update(self)`, is the act of sending the entire `self` object to the observer.  The push technique send a great deal of additional information regarding the changes `#update(self, old_salary, new_salary)` or `observer.update_salary(self, old_salary, new_salary)`.

The observer pattern is so common, Ruby also comes with its own observer module that can be found in the [standard library](http://ruby-doc.org/stdlib-2.0.0/libdoc/observer/rdoc/Observable.html). 

##<a name="composite"></a>Composite

Composite pattern is a combination of multiple smaller elements coming together to make one larger element. This forms a tree-like structure, the starting point at the top is the component, the very bottom of the tree are leaf classes, and the classes that fill the between are composite classes.

````ruby
class BuildHouse
  def initialize
    @build_walls = BuildWalls.new
  end
end
  ### roof and other items have been omitted for brevity ###
class BuildWalls
  def initialize
    @sub_tasks = [LayFoundation.new, PurchaseLumber.new]
  end
  def num_of_tasks
    total = 1
    @sub_tasks.each {|task| total += task.num_of_tasks}
    total
  end
end
class LayFoundation
  def num_of_tasks
    total = 1
  end
end
class PurchaseLumber
  def num_of_tasks
    total = 1
  end
end
````
Our overall goal is to build houses. We use the composite pattern to create a house, but we break this up into smaller individual classes. In order to build a house, we need walls. Walls need a foundation on which to stand and the material of the walls needs to be purchased. `BuildWalls` is our composite class, its leaf classes are `LayFoundation` and `PurchaseLumber`. It needs these leaf classes in order to complete its own task. It is also being depended on by our component class, `BuildHouse`. All these classes work together to help `BuildHouse` accomplish its function. The only actual functionality we have in our example is asking how many tasks in total our classes have. Each leaf task is 1, if we ask our `BuildWalls` class about how many tasks it has, the answer would be 3 (including itself). In a real setting, these methods would get broken out into their own class/module such as `Task` and inherited or included, then written over as needed.

##<a name="iterator"></a>Iterator

Iterators pattern will provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

####External

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
####Internal

With internal iterators, the aggregate will push the code block to accept item after item.

````ruby
array.each {|e| **code block**}
````

Ruby iterators use the [Enumerable module](http://ruby-doc.org/core-2.3.0/Enumerable.html), more specifically, the `<=>` operator. The class you are iterating through needs some way for the iterator to compare values. If we were to create our own iterator, it would behoove us to include this mixin.

##<a name="command"></a>Command

The main idea of the Command pattern is to factor out the action code into its own object. It is the separation of concerns, from the code that does not change, to the code that does. 

In our example below, we could have additional command classes. `ShowCommand`, `HideCommand`, `SaveCommand`, `DeleteCommand`, but the code that will not change with all these different commands is brought out into the `UniveralButton` class.

````ruby
class ShowCommand
  ### @ivar will represent some sort of state ###
  def description
    "show " + @ivar
  end
  def execute
    ### Something will be shown ###
  end
end
class UniveralButton
  def initialize(command)
    @command = command
    @logger = File.open('/var/log/command_app.log')
  end
  def push_button
    @command.execute if @command
    @log.append @command.description
  end
end
button = UniversalButton.new(ShowCommand.new)
````

Using the command pattern also helps to log executed commands. Our `ShowCommand` is a class, so it should have some state information to add to our description, then it can be logged to a file for future use. Perhaps you do not want to execute the same command twice, a safeguarded code block may check our log file to see if the code has already been ran. Or could be used as an undo, knowing which command was executed last could help determine what to do in order to 'undo' that last function. The [Madeleine](https://github.com/ghostganz/madeleine) gem is a great example of doing just that.

##<a name="adapter"></a>Adapter

An adapter is an object that crosses the chasm between the interface that you have and the interface that you need. Our below example has a `EmployeeManager` class, that would typically take an instantiated object of the `Employee` class. But we have a transfer from France, we would need a way for our French employee to interface with the employee manager. The `FrenchEmployeeConverter` will do just that, it will take our French employee and use its interface to correspond with the changes it needs to make to be of use to the `EmployeeManager` class.

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
  def initialize(employe)
    @employe = employe
  end
  def name
    @employe.prenom
  end
  def position
    @employe.poste
  end
  def id
    @employe.ca
  end
end
````

Another easy way to do this, is to modify the class after its been instantiated, this will allow us to omit the adaptor class and use the original class as a regular `Employee` class.

````ruby
employee = FrenchEmployee.new

class << employee
  def name
    prenom
  end
  ### additional changes to follow ###
end
````

This technique works well if the changes are simple, and we have deep knowledge of what our class is doing. However, if the changes are complex or we do not have a great understanding of what our class is doing, its safer to use `FrenchEmployeeConverter` class for our adaptor.

##<a name="proxy"></a>Proxy

Proxy classes hide away the real object within themselves, we refer to these real object as the "subject". This pattern is another separation of concerns. In our example, our employee data is hidden behind a EmployeeProxy that verifies the current user is authorized to view the sensitive data regarding a particular employee.

````ruby
class Employee
  attr_reader :id, :name, :ssn
end
class EmployeeProxy
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
class EmployeeProxy
  def initialize(&creation_block)
    @creation_block = creation_block
  end
  def subject
    @subject ||= @creation_block.call
  end
  def name
    e = subject
    e.name
  end
end
````

Instead of instantiating a Employee class directly, we will pass in a proc object and use that code block when needed `EmployeeProxy.new(Employee.new)`. This lowers coupling and allows other ways of creating an employee subject, `EmployeeProxy.new(Employee.find(#))`.

##<a name="decorator"></a>Decorator

Our Decorator design pattern takes a Concrete Component(the "real" object) that implements the base functionality and passes it to our Decorator through a Component class. The Decorators act as specialty classes to give the base class additional functionality once instantiated. We are able to chain our decorators to give our `super_tom` object even more functionality.

````ruby
class BaseEmployee ### ConcreteComponent ###
  def initialize(employee_hash)
    @name = employee_hash
  end
  ### base set of behaviors ###
  def work
    "work"
  end
end
class EnhancedEmployee ### Component ###
  def initialize(employee)
    @employee = employee
  end
  ### forward bevaviors to the base set ###
  def work
    @employee.work
  end
end
class Accountant < EnhancedEmployee ### Decorator ###
  def initialize(employee)
    super(employee)
  end
  def organizes_files
  end
end
class Developer < EnhancedEmployee ### Decorator ###
  def initialize(employee)
    super(employee)
  end
  def write_code 
  end
end
class DevOps < EnhancedEmployee ### Decorator ###
  def initialize(employee)
    super(employee)
  end
  def fix_server
  end
end
tom = Accountant.new( BaseEmployee.new(hash) )
super_tom = DevOps.new( Developer.new( BaseEmployee.new(hash) ) )
````
Another way to to get the decorator behaviors into out base class is to use modules. We would extend our instantiated class to include a Decorator if it has been defined as a module:

````ruby
module Developer
  ### functionality here ###
end
tom = BaseEmployee.new
tom.extend(Developer)
````
Now our tom object will have the additional functionality from our Developer module.

##<a name="singleton"></a>Singleton

The purpose of a singleton pattern is to avoid passing an object all over the place. Its a method that is called directly on the class itself, instead of an instantiated object of the class. Each way is a valid approach to creating a class method. 

````ruby
class Network
  def self.terminate_all
  end
  def Network.terminate_all
  end
  class < self
    def terminate_all
    end
  end
end
def Network.terminate_all
end
Network.terminate_all
````

We could instantiate one object to pass around under our class methods. This object would be available anywhere the `Logger` class is available. However, we have to take precaution not to create additional objects with this type of pattern, there can be one, and only one instance of the singleton class. The `private_class_method :new` will ensure we do not instantiate any additional objects from this class.

````ruby
class Logger
  @@instance = Logger.new
  def self.instance
    @@instance
  end
  private_class_method :new
end
````

The above code is so common, that Ruby has the `singleton` module for it. Just `require 'singleton'` and `include Singleton` inside our class and we would be able to omit the above mentioned code. The only difference is the module lazy instantiates our `@@instance = Logger.new` class variable, where our example eagerly instantiates it.

A singleton has a strong resemblance to the global variable, this give it the possibility to become tightly coupled with other sections of your program, there may also be difficulties locating where you have implemented the singleton when it could be in an arbitrary sections of code. The application of a singleton only works when you are modeling something that instantiates only once. 

Testing a singleton pattern may be difficult due to its global variable behaviors, to fix this, we can break out our methods into a base class where we are able to instantiate and test our methods. And then have a separate class that inherits those methods for our implementation of our singleton.

````ruby
class SimpleLogger
end
class SingletonLogger < SimpleLogger
  include Singeton
end
````

##<a name="factory"></a>Factory

Factory patterns are very similar to the template patterns. It takes a base class and sends it to a creator class which will create multiple instance of the base/product class. The creator is the class that contains our factory methods, the products are the classes that we are creating. In our example, we create the Zoo class and define our common set of methods, then our specialized classes `PenguinZoo` will inherit these methods and add its own specialized methods.

````ruby
class Zoo # creator
  def initialize(num, animal_type)
    @animals = []
    num.times { @animals << create_animal }
  end
end
class PenguinZoo < Zoo
  def create_animal
    Penguin.new
  end
end
class Penguin # product
end
````

We could go even further and create abstract factories. Where its the classes job to create compatable sets of objects. 

````ruby
class ArcticZooFactory # abstract factory
  def new_animal
    Penguin.new
  end
  def landscape
    Iceberg.new
  end
  def food
    Fish.new
  end
end
class Zoo
  def initialize(animal_num, landscape_num, exhibit_type)
    @exhibit_type = exhibit_type
    @animals = []
    animal_num.times { @animals << @exhibit_type.new_animal }
    ## create additional state from ArcticZooFactory ##
  end
end
````

This allows grouping of objects that have common properties. We could create other abstract factories such as `SafariZooFactory`, `TropicalZooFactory`, and `DesertZooFactory`. All would contain their respective classes. 

##<a name="builder"></a>Builder

A builder class takes charge of assembling all of the components of a complex object. Its purpose is to ease the burden of creating complex objects. We no longer have to know the specifics of a certain class, we just ask the builder for what we need. Builders are less concerned with picking the right class and more focued on helping you configure your object. Our below builder has a `#reset` method that allows us to reuse the builder. 

````ruby
class Shell
end
class Cheese
  def initialize(type)
    @type = type
  end
end
class Meat
  def initialize(type)
    @type = type
  end
end
class Taco
  def cheese_layer(cheese)
    @cheese << cheese
  end
end
class TacoBuilder
  attr_reader :taco
  def initialize
    @taco = Taco.new
  end
  def reset
    @taco = Taco.new
  end
  def add_shell
  end
  def add_meat(type="beef")
    @taco.meat = Meat.new(type)
  end
  def add_cheese(type)
    @taco.cheese_layer << Cheese.new(type)
  end
  def add_salsa(spicy=true)
  end
  def taco
    raise "no shell" if @taco.shell.nil? == true
  end
end
````

Our builder class will allow us to use constraints when we attempt to retrieve our `taco` object. In our example above, we will throw an error if there is no shell on hour taco. Also, notice the `#reset` method, this allows us to reused the builder after creating an instance of `Taco`.

##<a name="interpreter"></a>Interpreter

Parser reads in the program text and produces a data structure called abstract syntax tree. The abstract syntax tree has terminals and nonterminals, much like leaf and a parent. 

````ruby
class
end
class All
  ### retrieves everything ###
end
class Not
  def initialize(expression)
    @expression = expression
  end
  def evaluate
    All.new - @expression
  end
end
class Writable
  ### retrieves only writable objects ###
end
writable = Writable.new.evaluate
read_only = Not.new( Writable.new ).evaluate
````

Typically we would use the `All` class to find all our objects, or `Writable` to just find the writable files, but now with have a nonterminal `Not` class that we could use to negate whatever we pass into it as an argment. The `Not` class will use two terminal classes, our `All` and `Writable` to remove all writable objects from the all group. This is a brief example, but we could also include other classes such as `Or` and `And`. These nonterminals could use other combinations of terminal classes such as `Readable` and `Executable`, giving us a larger tress structure that we could traverse.

````ruby
class Parser
  302
end
````



````ruby
class Expression
  def |(other)
    Or.new(self, other)
  end
  def &(other)
    And.new(self, other)
  end
  def writable
    Writable.new
  end
  def but_not(arg) # avoiding collision with Rubys not operator
    Not.new(arg)
  end
  ### We can use this for all our classes ###
end
( writable & but_not(executable) ) | find_filename("*.txt")
````

