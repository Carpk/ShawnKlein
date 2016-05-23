---
layout: post
cover: 'assets/images/railroad_fog.jpg'
title: Rails File Structure
date:   2016-05-21 20:24:00
tags: unix/linux 
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

Rails has quite a few directories for breaking up and organizing our code. We are going to briefly explore what files belong in these directories, and how to best utilize them.

Lets start going over some of Rail's naming conventions. If we have a filename `foo_bar.rb`, the class or module that resides in that file needs to have the name `FooBar`. If it does not, Rails will not auto load it properly. 

````ruby
# app/models/foo_bar.rb
class FooBar
end
````
If we are using namespacing such as `Products::Apples`, our `apple.rb` file needs to be in a `/products` directory.

````ruby
# app/models/products/apple.rb
module Products
  class Apples
  end
end
````

Rails should autoload the below mentioned directories, if it does not, for whatever reason, you can tell rails to load a particular directory by specifying it in our application file.

````ruby
# /config/application.rb
config.autoload_paths << Rails.root.join('lib')
````

### Directories

#### /app/controllers

Directory for controller files. Files that put the C in MVC. These files are responsible for orchestrating the model and views. 

###### /app/controllers/concerns

Concerns are modules that can be used across controllers. This is helpful to DRY up your code by implementing reusable functionality inside the directory. The naming convention is module_name.rb.

#### /app/models

If a model represents a resource that is instantiated and manipulated by your application it goes here. These are models that are responsible for persisting data from your application to the database.

###### /app/models/concerns

Files in concerns should encapsulate both data access and domain logic about a certain slice of responsibility. These are modules for code that share a set of cross-cutting concerns from our models. Used to help DRY up code. And gives us the ability to properly resolve module dependencies using [require 'active_support/concern'](http://api.rubyonrails.org/classes/ActiveSupport/Concern.html).

#### /lib

lib directory is where all the application specific libraries goes. Application specific libraries are re-usable generic code extracted from the application. Think of it as an application specific gem.

###### /lib/assets

This file holds all the library assets, meaning those items (scripts, stylesheets, images) that are not application specific.

###### /lib/tasks

The application’s Rake tasks and other tasks can be put in this directory. Rake tasks mentioned here are required by the app’s Rakefile.

#### /vendor 

This is where the third-party vendor files go, such as javascript libraries (Angular) and CSS files (bootstrap), among others. Files added here will become part of the asset pipeline automatically.


