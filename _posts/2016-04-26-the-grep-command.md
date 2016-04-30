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

The grep command is without a doubt currently my favorite command. Lets say we are writing a large complex application, and we have some method that we don't know who calls it, or some code that is passed in and we have no idea from where or why. We can easily locate our elusive method call with `grep -r my_method`. The `-r` option will recursively search the contents of all the files in and below our current directory. We can also call this command as `rgrep search_pattern`.

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

If we have no need to traverse multiple files and subfolders, we can specify a specific file by using grep in its most basic form, `grep pattern filename.txt` we follow the `grep` command with the pattern to match `pattern` and then the file to search, `filename.txt` in this case.

````bash
~$ grep require load_paths.rb
require 'bundler'
````

One of the more useful options is to show the line number of our match. We would accomplish this by adding `-n` after `grep`. 

````bash
~$ grep -n task Rakefile
4:require "tasks/release"
5:require 'railties/lib/rails/api/task'
8:task :build => "all:build"
11:task :prep_release => "all:prep_release"
  ### don't worry, there was a lot more ###
````
We can call options by specifying either single character `-i` or writing the full name `--ignore-case`.

<table>
  <tr>
    <th>option</th>
    <th>full name</th>
    <th>description</th>
  </tr>
  <tr>
    <td>`-i`</td>
    <td>`--ignore-case`</td>
    <td>ignore case distinctions</td>
  </tr>
  <tr>
    <td>`-v`</td>
    <td>`--invert-match`</td>
    <td>display everything except our pattern match</td>
  </tr>
  <tr>
    <td>`-c`</td>
    <td>`--count`</td>
    <td>display only the number of line matches</td>
  </tr>
  <tr>
    <td>`-n`</td>
    <td>`--line-number`</td>
    <td>prefix line with its line number match</td>
  </tr>
</table>

And there are quite a few more options that come with `grep`, these can be found by using the `man grep` or `grep --help` commands.

