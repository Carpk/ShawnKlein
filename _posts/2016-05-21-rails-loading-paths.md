---
layout: post
cover: 'assets/images/railroad_fog.jpg'
title: Rails Loading Paths
date:   2016-04-26 20:24:00
tags: unix/linux 
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

### Rails loading paths

Lets start going over some of Rail's naming conventions. If we have a filename `foo_bar.rb`, the class or module that resides in the file needs to have the name `FooBar`. If it does not, Rails will not auto_load it properly. 

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

### /app/models

Your models are responsible for persisting data from your application to the database. 
Models act as object-relational mappers to the database tables that hold the data.

### /app/models/concerns

https://signalvnoise.com/posts/3372-put-chubby-models-on-a-diet-with-concerns

These concerns encapsulate both data access and domain logic about a certain slice of responsibility.


### /lib

lib directory is where all the application specific libraries goes. Application specific libraries are re-usable generic code extracted from the application. Think of it as an application specific gem.

###### /lib/assets

This file holds all the library assets, meaning those items (scripts, stylesheets, images) that are not application specific.

###### /lib/tasks

The application’s Rake tasks and other tasks can be put in this directory. Rake tasks mentioned here are required by the app’s Rakefile, which we’ll see below.

### /vendor 

This is where the third-party vendor files go, such as javascript libraries and CSS files, among others. Files added here will become part of the asset pipeline automatically.


