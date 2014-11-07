---
layout: blog
category: blogs
title: 'Hello World: How to write a tech blog.'
date: '2014-11-06'
author: Kgao
tags: 'Github,Marketdown'
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.

#Step 1. Determine your motivation for writing a technology blog. 

This will play an important role in determining which topics you write about and how much depth and background or supporting information you provide. 

If you just want to get show off some code, you can put it in a html bash using:
{% highlight bash %}
$ print: What? You start to write some code now?
{% endhighlight %}

A hobby, an expert, for fun?
Lets assume we have a good motivation beside earning money.

#Step2. Decide what to write about.

That's always the most difficult part. What did I just wrote?

Plan your topics for the next 3 months ahead of time so you're prepared and have plenty of time to research them. If you're serious about blogging, you might want to have the entire 3 months of content written ahead of time so you can maintain a consistent number of posts while researching and writing more content. 

Again, highlight some ideas:
{% highlight bash %} 
$ print: You have to decided on your own!
{% endhighlight %}

#Step3. Do research.

Even if you know a lot about a subject, always make sure to do some quick research to verify what you're writing. If you are writing as an expert, provide more detailed research, as well as sources and links to other pages.

Here is some example for research:
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


#Step4. Set a tone for your blog.

Very important!

Blogs are casual, opinionated authored articles and usually don't include a lot of complicated terms and language. Even if you're an expert, try to explain things in terms that a layman will understand.

