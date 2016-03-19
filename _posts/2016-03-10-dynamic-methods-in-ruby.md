---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Dynamic methods in Ruby
date:   2016-03-10 10:18:00
tags: ruby
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---
There are three main ways to get Ruby to dynamically generate code for us. These are ways that Ruby will generate code for a method when it simply does not exist yet.

In our examples, we will be defining code in a Library class, which could have an ivar called `@books` as an array of Book objects.

###Send

The `#send` method is pretty much at the center of all our dynamic methods. It takes a symbol, and passes it to the calling class where it searches that class and all ancestors for that method to execute. For our example, lets say our Book objects have `attr_reader :title` enabled.

```
def sort_books_by(attr)
  @books.sort_by {|p| p.send(attr)}
end
```

And our `library` object would reference this method as:

`library.sort_books_by('title')`

###Define method

`#define_method` works by taking a block and defining that instance method on the receiver. So you are are creating as many methods are there are in the `books` array.

```
@books.first.instance_variables.each do |ivar|
  type = ivar[1..-1]
  define_method("sort_books_by_" + type) do
    @books.sort_by {|b| b.send(type)}
  end
end
```

If the above code was in a Libray class, you could call one of the defined methods as:

`library.sort_books_by_author`

###Method missing

Probably the most dangerous of way to dynamically create methods, Ruby looks for a method in the current class, then checks all of that classe's ancestors for the called method. If it does not find it, it calls `#method_missing` on the BasicObject class. We are redefining that method to allow our `sort_books_by_index` to be caught, and have the `index` attribute to be passed using `#send`.  

```
def method_missing(method, *args)
  attr = method.split('_').last
  super unless self.books.first.respond_to?(attr)
  self.books.sort_by{ |b| b.send(attr) }
end
```

And the below code will run without any problems:

`library.sort_books_by_index`

