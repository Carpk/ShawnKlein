---
layout: post
cover: 'assets/images/sloping_mountains.jpg'
title: Creating a DSL
date:   2016-04-05 10:18:00
tags: ruby coding
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

Rake, RSpec, and ActiveRecord are all examples of a Domain Specific Language(DSL). There are Internal DSLs, which was will explore first, and then External DSLs, which is when the parser/interpreter and the programs written in the DSL are completely distinct. For our internal DSL example, we have a mock logging utility that will inform us once we have more than 5 blog posts. It simply consists of a name and code block. The end result is to have the code block return a boolean. `true` if we have more that 5 blog posts, `false` if we do not.

````ruby
### events.rb
event "we are cranking out blog topics" {
  blog_posts = ### read from db ###
  blog_posts.count > 5
}
````
Now we need a way for Ruby to read our event. To utilize our code block and make the distinction if in fact we do have more than 5 blog posts at the time this event has been ran.

````ruby
def event(name)
  puts "ALERT: #{name}" if yield 
end
````

Our above method will be loaded when we read the `events.rb` file, and each event will get treated as a method call `event("we are cranking out blog topics") { **code block** }`. The description will be passed in as an argument, and when our event's code block returns `true`, the argument will be passed in allowing our output to be created, resulting in: `Alert: we are cranking out blog topics`. We could continue with creating events as long as we have variables to check. And we, of course, are not limited to writing things to the command prompt. As long as we have a defined method that promotes some functionality, we can have have our DSL calling those functions with the appropriate arguments.  

````ruby
event "site is recieve lots of views" {
  web_traffic.views > 15
}
event "someone shared a post" {
  twitter.shared? || facebook.shared? || g_plus.shared?
}
log "an error occured" {
  log.write(msg, errors.all) if errors?
}
````

An external DSL might be a little more user friendly, as it doesnt have be written using valid Ruby syntax, because we will be building a parser from scratch. It might look something such as `report warning low toner`, that is definetly not Ruby syntax! Lets build a parser for it.

````ruby
class ReportParser
  CSV.each_line("daily_log") {|line| actions(line.split)}
  def actions(cmds)
    case cmds.first
    when "report"
      Email.send_email(type: cmds[1], message: cmds[2,-1].join(" ")
    end
  end
end    
````
Our parser class takes a file, parses over each line, breaks it up into an array using `#split` and sends it to a method that uses `case` on the first element of our array to find its match at `when "report"`. Then it executes the other series of commands that entails what our DSL should do. A simpliler way to achieve the first step would be to use `#send` on `self` for the first of our string of commands `self.send(cmds.first, cmds)`. An external DSL simply models its interface to something beside the language that is parsing it.
