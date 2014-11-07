---
layout: blog
category: blogs
title: 'Hello Blog Test 1 : Give me a longer name'
date: '2014-11-06'
author: Kgao
tags: 'Github, io'
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference
about this matter.

#Discarding output

If you just want to get rid of the output, you can redirect to void using:
{% highlight bash %}
$ python foo.py > /dev/null
{% endhighlight %}


When running a application in a non dev environment, you certainly want to log messages and errors to a file rather than just
displaying to the console.  
Let's assume we have a python script called `foo.py`.

#Redirect standard output to the file app.log

Standard redirection is done with a single >. This will replace the file each time your restart your script. Two > will append
to the file.

{% highlight bash %}
#Erase current log content
$ python foo.py > app.log
#Append to current log content
$ python foo.py >> app.log
{% endhighlight %}

> Note that you can simple empty a file with:

{% highlight bash %}
#Erase current log content
$ > app.log
{% endhighlight %}

#Redirect error output to the file err.log

Our test python script `foo.py`:
{% highlight python %}
import logging
print 'Info message is logged'
try:
    1/0
except Exception, e:
    logging.exception(e)
{% endhighlight %}

Error is accessible with `2` while standard output is `1`, so you can redirect error with:
{% highlight bash %}
#Sdtout to app.log and errors to err.log
$ python foo.py > app.log 2> err.log
#Equivalent to: python foo.py 1> app.log 2> err.log

$ cat err.log 
ERROR:root:integer division or modulo by zero
Traceback (most recent call last):
  File "foo.py", line 4, in <module>
    1/0
ZeroDivisionError: integer division or modulo by zero

$ cat app.log 
Info message is logged
{% endhighlight %}

#Redirect standard and error outputs to the same file all.log

{% highlight bash %}
#We redirect the file descriptor of error 2 to the file descriptor of standard output &1
$ python foo.py > all.log 2>&1

#Easier syntax for Bash greater than 4.0
$ python foo.py &> all.log
{% endhighlight %}