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
book.instance_variables.each

end
```

###Method missing

```
def method_missing(method_name, *args)
  attr = method.split('_').last
  super unless book.respond_to?(attr)
  self.sort_by{|b| b.attr}
end
```

`library.sort_books_by_index`

