---
layout: post
cover: 'assets/images/mountainous_lake.jpg'
title: Creating a Cronjob
date:   2016-05-11 10:18:00
tags: linux/unix devops
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---


Cron is a system daemon that executes tasks at designated times. Crontab files are user specific, and can executed with admin privledges. Look for our current set of cronjobs using the `-l` option.

````bash
~$ crontab -l
no crontab for user
````

We will edit a crontab file for our current user, using `crontab -e`. The `-e` option states we are editing, and add the following line to the bottom of the crontab page. Make sure you replace `user` with your user name:

````
* * * * * echo "cronjob minute test" >> /home/user/log
````

This will write "cronjob minute test" to a file named 'test' in our user's home directory. And it will do this every minute, of every hour, of every day/month.

And for our Ruby aficionados, we need certain environment settings to run `ruby script.rb`, so first we need the path of our ruby executable, this can be found in `/usr/local/bin`, `/usr/bin`, or `/bin`. We state the ruby execuatable and its path, followed by our ruby script, and then we shovel the stdout into our log file. `2>&1` will redirect any stderr to the same file that stdout is pointing to. Meaning it will include any errors to our log file. We can omit this, but then we will not be able to tell if something is wrong with our script.

````
* * * * * /usr/bin/ruby /home/user/task/script.rb >> /home/user/task/log 2>&1
````

When configuring the time our script should be ran, we enter it as a time and date. Say we have a cronjob that looks like `15 7 3 1 * /usr/bin/ruby /home/user/script.rb >> /home/user/log`

<table>
  <tr>
    <th>minute</th>
    <th>hour</th>
    <th>day</th>
    <th>month</th>
    <th>dayOfWeek</th>
  </tr>
  <tr>
    <td>15</td>
    <td>7</td>
    <td>3</td>
    <td>1</td>
    <td>*</td>
  </tr>
</table>

This will run our script at 7:15am on the 3rd of January. While that may be a long way off, we can also schedule our scripts to be ran on a weekly basis. 

````
30 3 * * 2 /usr/bin/ruby /home/user/script.rb >> /home/user/log
````

This would run our script at 3:30am every Monday morning. We are able to run our script after a certain succession of time by specifying the intervals with `*/`. If we need our script run every 10 minutes, we would use:

````
*/10 * * * * /usr/bin/ruby /home/user/script.rb >> /home/user/log
````

Finally, we use `crontab -r` to remove our crontab. After executing this command and checking cronjobs with `crontab -l`, we will be informed with `no crontab for user`. As always, you can find more info using `man crontab`, `man 5 crontab`, and `man 8 cron`.


