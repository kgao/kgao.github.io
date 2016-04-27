---
layout: blog
category: blogs
title: 'Build Scala In Play Framework'
date: '2014-11-07'
author: Kgao
tags: 'Play, Scala'
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.



Play Framework makes it easy to build web applications with Java & Scala.
Play is based on a lightweight, stateless, web-friendly architecture.
Built on Akka, Play provides predictable and minimal resource consumption (CPU, memory, threads) for highly-scalable applications.

Example is built on mac os x.
Scala Project in Play Framework.

## Install Scala:
Install: scala-2.10.3
horstmann.com/scala/install

{% highlight bash %}
$ brew install scala
{% endhighlight %}

Then, add Scala to Path:
{% highlight bash %}
$ PATH=/opt/local/bin:/Users/{yourName}/workspace/scala-2.10.3/bin:/opt/local/sbin:$PATH
## or
$ export PATH=$PATH:/Users/{yourName}/workspace/scala-2.10.3/bin/
{% endhighlight %}

Then use $scala, you shall enter the world of scala > 
{% highlight bash %}
$ scala
scala > 


{% endhighlight %}


## Install play framework:

You can use the following command:
{% highlight bash %}
$ brew install --devel play
{% endhighlight %}


You can check latest version by:
{% highlight bash %}
$ brew update
$ brew info play
{% endhighlight %}

Or go to the formula file:

https://github.com/mxcl/homebrew/blob/master/Library/Formula/play.rb


Then, add Play to Path:
{% highlight bash %}
$ export PATH=$PATH:/Users/{yourName}/Documents/play-2.1.1/
##  or
$ export PATH=$PATH:/Users/{yourName}/.../play-2.1.3/
{% endhighlight %}


## Play: Create new project:

Step1 Build project:
{% highlight bash %}

$ play new KevFirstPlay

{% endhighlight %}

Step2 Open project:
{% highlight bash %}

$ cd kevFirstPlay

{% endhighlight %}

Step3 Launch: 
{% highlight bash %}

$ play

{% endhighlight %}
Then you lanuch your first play app!

You can also use compile/run to start your project:
{% highlight bash %}
[My first application] $ compile
[My first application] $ run
{% endhighlight %}

Step4: Review in browser after server is running:

http://localhost:9000/

(Server started, use Ctrl+D to stop and go back to the console)



## Play Plus IDE: (i.e. Eclipse)

Play provides a command to simplify Eclipse configuration. To transform a Play application into a working Eclipse project, use the eclipse command:

without the source jars:
{% highlight bash %}
[My first application] $ eclipse
{% endhighlight %}
If you want to grab the available source jars (this will take longer and it’s possible a few sources might be missing):
{% highlight bash %}
[My first application] $ eclipse with-source=true
{% endhighlight %}
Note if you are using sub-projects with aggregate, you would need to set skipParents appropriately:…or from the play console, type:
{% highlight bash %}
[My first application] $ eclipse skip-parents=false
{% endhighlight %}
Then import from IDE.

Tip: Do not commit Eclipse configuration files when you work in a team!





