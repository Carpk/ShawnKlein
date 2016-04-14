---
layout: post
cover: 'assets/images/sloping_mountains.jpg'
title: Creating a DSL
date:   2016-04-05 10:18:00
tags: ruby 
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

Rake, RSpec, and ActiveRecord are all examples of a Domain Specific Language(DSL). We have a mock logging utility that will inform us once we have more than 5 blog posts. It simply consists of a name and code block. The end result is to have the code block return a boolean. `true` if we have more that 5 blog posts, `false` if we do not.

````ruby
### events.rb
event "we are cranking out blog topics" {
  blog_posts = ### read from db ###
  blog_posts > 5
}
````
Now we need a way for Ruby to read our event, If it returns `true`, bells and whistles go off. If `false`, nothing happens.

````ruby
def event(name)
  puts "ALERT: #{name}" if yield 
end
````

Our above method will be loaded when we read the `events.rb` file, and each event will get treated as a method call `event("we are cranking out blog topics") { **code block** }`. And of course our event will return `true`, so we get the `Alert: we are cranking out blog topics` posted in our terminal.

````ruby
event "site is recieve lots of views" {
  web_traffic.views > 15
}
event "someone shared a post" {
  twitter.shared? || facebook.shared? || g_plus.shared?
}
````
