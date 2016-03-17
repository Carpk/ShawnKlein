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
There are three main ways to get to Ruby dynamically generate code for us.

###Send

```
def sort_books_by(attr)
  @books.sort_by {|p| p.send(attr)}
end
```

`library.sort_books_by('title')`

###Define method

```
books.first.instance_variables.each do |ivar|
  type = ivar[1..-1]
  define_method("sort_books_by_" + type) do
    books.sort_by {|b| b.send(type)}
  end
end
```

###Method missing

```
def method_missing(method, *args)
  attr = method.split('_').last
  super unless self.books.first.respond_to?(attr)
  self.books.sort_by{ |b| b.send(attr) }
end
```

`library.sort_books_by_index`

