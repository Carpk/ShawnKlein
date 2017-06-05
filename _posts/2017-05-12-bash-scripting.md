---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Bash Scripting
date:   2017-05-12 9:27:00
tags: unix/linux devops coding
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

One of the most fundemental steps in working in GNU/Linux is writing shell scripts. This post will walk through some basic scripting and revisited upon to included some more advanced topics. 

#### Directories and Permissions

Use `touch test.sh` to create our file. If we attempt to run our file, we may see an error such as:

````bash
$ test.sh
test.sh: command not found
````

The file needs to be called with `./` to denote that the file is in the current directory. The default behavior is to look in the bin folders of your `$PATH`. These can be seen with `echo $PATH`. For our project, using `./` with our file in a `/home` or `/projects` folder is acceptable. Once we make the nessecary changes, another problems raises:

````bash
$ ./test.sh
bash: ./test.sh: Permission denied
````
We wouldnt want just any scripts to be able to run on out machine, so we need to change permissions to allow us to run our test script:

````bash
$ chmod 777 test.sh
````
Last thing this section covers is appending a shebang `#! /bin/bash` to the top of our bash script. This tells the shell which application to run our script in. 

````bash
#! /bin/bash

echo "Hello World!"
````
We should now have a runnable shell script.

#### Variables

Assiging variables is easy enough with `=` and no spaces inbetween. All-caps variable name suggests environment variable, or read from a global config file. Lower-case is often used for local variables along with the use of underscores to separate components.

Reading variables requires the use of dollar sign `$`, and could often be preceeded with `{}` to encase the variable to clarify to the parser when the variable stops and other text begins.

````bash
GLOBALVAR='foo';
local_var='bar';

echo $GLOBALVAR$local_var
echo ${GLOBALVAR}${local_var}
````



#### Whiptail

````bash
#! /bin/bash

whiptail --title "welcome" --msgbox "This is in a whiptail message box." 10 60
````

````bash

````




