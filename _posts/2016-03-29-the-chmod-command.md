---
layout: post
cover: 'assets/images/railroad_fog.jpg'
title: The Chmod Command
date:   2016-03-29 10:18:00
tags: unix/linux 
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

The chmod command is an abbreviation of change mode. It is used to change access permissions to files directories. If we were to `ll`(short for `ls -l`) from out home directory, we many see something like this:

````
drwxr-xr-x  56 shawn users   4096 Apr  1 12:32 .
drwxr-xr-x   4 root  root    4096 Mar  2 12:30 ..
drwxr-xr-x   3 shawn users   4096 Mar 31 09:57 Desktop
drwxr-xr-x   6 shawn users   4096 Mar 30 16:34 Documents
drwxr-xr-x  18 shawn users   4096 Mar 31 23:05 Downloads
drwxr-xr-x   2 shawn users   4096 Jan 27 20:13 Music
drwxr-xr-x   8 shawn users  20480 Mar 29 12:27 Pictures
drwxr-xr-x  17 shawn users   4096 Mar 29 09:19 Projects
drwxr-xr-x   2 shawn users   4096 May 10  2015 Public
drwxr-xr-x  12 shawn users   4096 May 10  2015 .rbenv
-rw-r--r--   1 shawn users      8 Nov 25 16:56 .rspec
drwxr-xr-x   7 shawn users   4096 Nov 19 23:09 Templates
drwxr-xr-x   4 shawn users   4096 Jan 11 18:59 Videos
drwxr-xr-x   4 shawn users   4096 Apr  1 12:32 .vim
-rw-------   1 shawn users  25347 Apr  1 12:32 .viminfo
-rw-r--r--   1 shawn users    138 Feb 16 12:52 .vimrc
````

The first set of characters `drwxr-xr-x` are the permissions of that file/folder. The first charater `d` states that this is a directory, the next three `rwx` (which stand for Read, Write, eXecute) are permissions for the owner, preceeding three `r-x` are permissions for the group, and final three `r-x` are permissions for others (not owner or member of group).

`drwxr-xr-x   3 shawn users` breakdown:

<table>
  <tr>
    <th>dir</th>
    <th>owner</th>
    <th>group</th>
    <th>others</th>
    <th>sub-dir</th>
    <th>owner name</th>
    <th>group name</th>
  </tr>
  <tr>
    <td>`d`</td>
    <td>`rwx`</td>
    <td>`r-x`</td>
    <td>`r-x`</td>
    <td>`3`</td>
    <td>`shawn`</td>
    <td>`users`</td>
  </tr>
</table>

The first character `d` informs us this is a directory. The Owner, is quite obviously the owner of the file object,which in our example is `shawn` has Read, Write, and eXecute permissions. Group is set with group permissions, so any one in the `users` group would have those sets of permissions applied to them. The `users` group has Read and eXecute permissions. And finally is the `others` which is anyone besides the owner or members of the applied group, which they also have Read and eXecute permissions. sub-dir shows us how many directories that this path holds.

Now that we understand how to determine what permissions are applied to a file object, we can change them using the `chmod` command.

The chmod command has numerical values attached to each permission. Such as `execute` has a value of `1`, and `read` has a value of `4`. You can add these values so that `5` would equate to `read and execute` permissions. 

<table>
  <tr>
    <th>#</th>
    <th>Permission</th>
    <th>rwx</th>
  </tr>
  <tr>
    <td>7</td>
    <td>read, write and execute</td>
    <td>rwx</td>
  </tr>
  <tr>
    <td>6</td>
    <td>read and write</td>
    <td>rw-</td>
  </tr>
  <tr>
    <td>5</td>
    <td>read and execute</td>
    <td>r-x</td>
  </tr>
  <tr>
    <td>4</td>
    <td>read only</td>
    <td>r--</td>
  </tr>
  <tr>
    <td>3</td>
    <td>write and execute</td>
    <td>-wx</td>
  </tr>
  <tr>
    <td>2</td>
    <td>write only</td>
    <td>-w-</td>
  </tr>
  <tr>
    <td>1</td>
    <td>execute only</td>
    <td>--x</td>
  </tr>
  <tr>
    <td>0</td>
    <td>none</td>
    <td>---</td>
  </tr>
</table>

Using these numerical values, we are able to apply permisions quickly and easily. You have a `share.txt` file that you want to give everyone read, write, and execute permissions, you cand do this with the `chmod` command, value for owner `7`, value for group `7`, and value for others `7`, followed by the filename:

`chmod 777 share.txt`

How about rwx for owner, rx for group, and read for others:

`chmod 754 share.txt`

You can view that these permissions have taken place using `ls -l share.txt` command.

While the `chmod` command offers a handful of options, the only one we will be covering is the recursive `-R` option. The recursive option allows us to apply the permissions to the current folder and all of its subdirectories.

`chmod -R 775 my_folder/`

This will apply rwx to owner and group, and rx to others, in for this folder and all of its sub folders/files.
