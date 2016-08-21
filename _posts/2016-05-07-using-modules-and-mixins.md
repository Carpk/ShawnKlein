---
layout: post
cover: 'assets/images/crashing_waves.jpg'
title: Using Modules and Mixins
date:   2016-05-07 10:18:00
tags: ruby coding
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

Ruby gives us two ways to mixin a module, the `extend` and `include` methods.

* __include__ includes the additional methods
* __extend__ extends the class

We cannot use the typical `self` to create mixin class methods. Using `self` will refer to the module directly. But this is still a great way to have methods that are purely functional.

````ruby
class MyModule
  def self.klass_method; "I'm a class method from MyModule!"; end
    # same as
  def MyModule.klass_method; "I'm a class method from MyModule!"; end
end
MyModule.klass_method # ~> "I'm a class method from MyModule!"
````

Instead, we will use the `extend` method to mixin the module's methods. We could also use `include` if it is including in a class method context.

````ruby
module KlassMethods
  def my_method; "I'm a Klass Method"; end
end
class KlassBase
  extend KlassMethods
end
KlassBase.my_method # ~> "I'm a Klass Method"
class AnotherKlassBase
  class << self
    include KlassMethods
  end
end
AnotherKlassBase.my_method # ~> "I'm a Klass Method"
````
This extends the class methods that are available to `KlassBase`. If we need to postpone the extension of `KlassBase`, we could use `KlassBase.extend(KlassMethods)` at the later date.

````ruby
module InstanceMethods
  def my_methods; "I'm an Instance Method"; end
end
class KlassBase
  include InstanceMethods
end
KlassBase.new.my_method # ~> "I'm an Instance Method"
````

Using `include` will include the module's methods into the class that is being used for instantiated method calls. We can also use `include` to include class methods by telling the module to extend the base class to its nested module of class methods using the `included` callback. The `included` callback is invoked whenever the current module is included into another class or module.

````ruby
module MyMethods
  def self.included(base)
    base.extend(MyKlassMethods)
  end
  def my_instance_method
    "I'm an instance method"
  end
  module MyKlassMethods
    def my_klass_method
      "I'm a class method"
    end
  end
end
````

To test a module's functionality, we need to instantiate an object in order to mixin our module. Once we have an instanitated object to pass around, we have something to run our tests against.

````ruby
  let(:dummy_klass) { Object.new { include MyMethods } }
````

Also note that our `Object` class has been extended to the module's class methods, this allows us to test class methods such as `Object.my_klass_method`. 


