---
layout: post
cover: 'assets/images/railroad_fog.jpg'
title: The Grep Command
date:   2016-04-26 20:24:00
tags: unix/linux 
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

The grep command is without a doubt currently my favorite command. Lets say we are writing a large complex application, and we have some method that we dont know who calls it, or some code that is passed in and we have no idea from where or why. We can easily locate our elusive method call with `grep -r my_method`. The `-r` option will recursively search the contents of all the file in and below our current directory. We can also call this command as `rgrep search_pattern`.

````bash
~$ grep -r FRAMEWORKS
tasks/release.rb:FRAMEWORKS = %w( activesupport activemodel activerecord actionview actionpack activejob actionmailer actioncable railties )
tasks/release.rb:(FRAMEWORKS + ['rails']).each do |framework|
tasks/release.rb:    (FRAMEWORKS + ['guides']).each do |fw|
tasks/release.rb:    (FRAMEWORKS + ['guides']).each do |fw|
tasks/release.rb:    (FRAMEWORKS + ['guides']).each do |fw|
tasks/release.rb:  task :build           => FRAMEWORKS.map { |f| "#{f}:build"           } + ['rails:build']
tasks/release.rb:  task :update_versions => FRAMEWORKS.map { |f| "#{f}:update_versions" } + ['rails:update_versions']
tasks/release.rb:  task :install         => FRAMEWORKS.map { |f| "#{f}:install"         } + ['rails:install']
tasks/release.rb:  task :push            => FRAMEWORKS.map { |f| "#{f}:push"            } + ['rails:push']
Rakefile:    FRAMEWORKS.each do |project|
Rakefile:  (FRAMEWORKS - %w(activerecord)).each do |project|
````
And we have a first hand look at how Rails builds it's framework tasks, if you are in rails's root directory.













