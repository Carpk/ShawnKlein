---
layout: post
cover: 'assets/images/forest_path.jpg'
title: Guide to Object Oriented Design
date:   2016-04-14 10:18:00
tags: ruby 
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

In a procedural language, behavior and data are separate. Data gets packages into variables and pass around to the behaviors. But an OO language combines them into what we refer to as an *object*. Objects have behavior and may contain data. They invoke behavior in one another by sending each other messages. For code to be agile, it must be easy to change. Change to meet future demands of our application.

Code that is easy to change:

* Changes have no unexpected side effects
* Small changes in requirements require correspondingly small changes in code
* Existing code is easy to reuse
* The easiest way to make a change is to add code that in itself is easy to change.

Code should be:

* Transparent: The consequences of change should be obvious in the code that is changing and in distant code relies upon it
* Reasonable: The cost of any change should be proportional to the benefits the change achieves
* Usable: Existing code should be usable in new and unexpected contexts
* Exemplary: The code itself should encourage those who change it to perpetuate these qualities

The best way to meet these criteria is to make sure your classes each have a single, well defined responsibility. Classes should do the smallest possible useful thing.

````ruby
class Gear
  attr_reader :chainring, :cog
  def initialize(chainring, cog)
    @chainring = chainring
    @cog = cog
  end
  def ratio
    chainring / cog.to_f
  end
end
puts Gear.new(52, 11).ratio # -> 4.72727272727273
puts Gear.new(30, 27).ratio # -> 1.11111111111111
````

The class above has two instance variables, and the only method uses both. It is a good representation of a simple class that has a single responsibility. We could determine its single responsibility by asking the class about its behaviors, “Mr. Gear, what is your ratio?” is a great question, “Mr. Gear, what is your tire size?” is not! If the gear class was able to respond to the tire size question, it has too many responsibilities. Responsibilities that may get caught up against other reasonable questions such as "what is your ratio?". We don't want our ratio to change when the tire size changes, we don't want a `ArgumentError: wrong number of arguments` error when we ask about the ratio and didn't provide a tire size. Good practice is to try and describe the class in one sentence.

####Cohesion: 
The sticking together of particles of the same substance. Classes have high cohesion when they are also of a single responsibility. When everything that the class does, is highly related to its purpose.

####Coupling:
The manner and degree of interdependence between software modules; a measure of how closely connected two routines or modules are; the strength of the relationships between modules.

Wrap instance variables in accessor methods instead of directly referring to them. Hide variables, even from classes that define them. `attr_reader` is an easy way to create a encapsulating method.

````ruby
class Gear
  attr_reader :chainring, :cog
  def ratio
    chainring / cog.to_f # <-- good
  end
  def ratio
    @chainring / @cog.to_f # <-- bad
  end
end
````

Changing the instance variables to methods allows for changes when the class changes. If we have to take into account a `tire` for some reason, we can make the changes very easily instead of having to change the `@cog` instance variable everywhere that its written.

````ruby
def cog
  tire? ? tire * @cog : @cog
end
````

This also helps in a more abstract way. Because it’s possible to wrap every instance variable in a method and to therefore treat any variable as if it’s just another object, the distinction between data and a regular object begins to disappear. While it’s sometimes expedient to think of parts of your application as behavior-less data, most things are better thought of as plain old objects.

Overly complex instance variables leads to multiple issues, the referencing code has to know more of the inner structure of the instance variable to access the data it needs. Code that received an instance variable that looked like `[[622, 20], [622, 23], [559, 30]]` would need to know that there is an inner array, then call upon those indexes for it values. Code such as `@nested_array.each {|array| tire_sizes << array[1]}` would have to be everywhere in order to access the array's data. The references are leaky, they would escape encapsulation, and eventually become a maintenance nightmare. It would be practical to use `Struct` class to organize our complex data. 

````ruby
Wheel = Struct.new(:rim, :tire)
def wheelify(data)
  data.collect {|cell| Wheel.new(cell[0], cell[1])}
end
def gear_inches
  ratio * wheel.diameter
end
Wheel = Struct.new(:rim, :tire) do
  def diameter
    rim + (tire * 2)
  end
end
````

Now we have an array of `Structs` that we could call `wheel.tire` on. Ruby defines Struct as “a convenient way to bundle a number of attributes together, using accessor methods, without having to write an explicit class.”. If the input changes, the only place to change the code is in this one place. This allows it to be more readable and intention revealing. We can also define `Struct` methods by using the syntax in the lower portion of the example. If our class has too many responsibilities, we can isolate them in a `Struct` class, and move them into their own class when we are good and ready.

````ruby
def gear_inches
  ratio * (rim + (tire * 2)) # bad
end
def gear_inches
  ratio * diameter # good
end
def diameter
  rim + (tire * 2) # good
end
def gear_inches
  ratio * Wheel.new(rim, tire).diameter
end
Wheel = Struct.new(:rim, :tire) do
  def diameter
    rim + (tire * 2)
  end
end
````

The `Wheel.new` instantiation in `#gear_inches` is another example of a dependency that we could change. If class name `Wheel` every changes, it must also change in our Gear class. When hard coded with `Wheel`, the `Gear` class can only calculate `gear_inches` using the `Wheel` class, it will not collaborate with any other object. The class doesn't need to know the name of the other class, It just needs an object that responds to `#diameter`. It would be better to pass in the `Wheel` class when we initialize the `Gear` class `Gear.new(52, 11, Wheel.new(26,1.5))`, then we could set this object as an instance variable and call our method on it. This is know as Dependency Injection, we are injecting our dependencies when we instantiate our class.

When we must have dependencies, we have to isolate them where it is easy to make minor changes, if the need were to ever arise. If we can not use dependency injection, we need to instantiate the class where it is easy to find.

````ruby
def initialize
  @foo = Foo.new
end
def bar
  @bar ||= Bar.new
end
````
It is the same with sending messages to other classes. When  we have multiple calls to and object, such as `wheel.diameter`, it is best practice to isolate these calls to its own method.

````ruby
def gear_inches
  ivar * wheel.diameter # bad if more of these calls
end
def gear_inches
  ### math ###
  foo = num * diameter # good
end
def diameter
  wheel.diameter
end
````

If we are unable to change a class, it is possible to wrap the class up in a module to have change the interface we have to work with. Our `GearWrapper` allows us to create a `Gear` instance using a hash instead of having multiple dependencies on the ordering of the argument. This makes our `GearWrapper` a type of factory. It is always ideal to wrap an external dependency in something that is owned by your own code.

````ruby
class Gear
  def initialize(cog, gear, sprocket, tire)
    ### assigns to respective ivars ###
  end
end
module GearWrapper
  def self.gear(args)
    Gear.new(args[:cog], args[:gear], args[:sprocket], args[:tire])
  end
end
````

It's best to focus on the messages, not the objects. If we fixate on the domain objects, we tend to coerce behavior onto them. Changing the fundamental design question from “I know I need this class, what should it do?” to “I need to send this message, who should respond to it?” is the first step in that direction. You don’t send messages because you have objects, you have objects because you send messages. Asks for what the sender wants instead of sending a message telling the receiver how to behave.


![sequence diagram](/assets/images/sequence_diagram.png)

In the first sequence diagram, `Trip` is telling `Mechanic` how to behave. It's very procedural code. The second sequence diagram is more object-oriented. `Trip` asks `Mechanic` to prepare a `Bicycle`. This lets `Mechanic` have a smaller public interface. And in the last, Trip knows nothing about Mechanic but still manages to collaborate with it to get bicycles ready. `Trip` merely tells `Mechanic` what it wants, which is to be prepared, and passes itself along as an argument. Then `Mechanic` knows the argument can respond to `#bicycles` and is able to preform its duties. Mechanic class is saying "Hey, I'm a bicycle mechanic, I expect to be given bicycles".

This way, we can populate the `Trip`'s array with certain set of objects and have a preparer that knows how to prepare how to interact with those objects. You can *extend* `Trip` without *modifying* it. `Trip` trusts the preparing class that it knows how to get what it needs.

Message chains like customer.bicycle.wheel.rotate occur when your design thoughts are unduly influenced by objects you already know. Your familiarity with the public interfaces of known objects may lead you to string together long message chains to get at distant behavior.Focusing on messages reveals objects that might otherwise be overlooked. When messages are trusting and ask for what the sender wants instead of telling the receiver how to behave, objects naturally evolve public interfaces that are flexible and reusable in novel and unexpected ways. Its not the class of the object that matters, its what the object *does*.

````ruby
class Trip
  attr_reader :bicycles
  def prepare(mechanic)
    mechanic.prepare_bicycles(bicycles)
  end
end
````


 






