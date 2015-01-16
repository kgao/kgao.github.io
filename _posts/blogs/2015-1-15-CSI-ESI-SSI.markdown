---
layout: blog
category: blogs
title: 'CSI vs SSI vs ESI: Concept and Usecase'
date: '2015-1-15'
author: Kgao
tags: 'CSI,SSI,ESI'
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.


#Problem:

We have a media site with tons of articles pages and readers.
They like us and they like to share those great articles to their social networks, like fb, twitter and pintrest...etc.
Nowadays its normal to showoff the popularity of a core product for each company.
So are we. We tend to add share counts on the page to show how popular they are and which is the most hot one for users.

i.e.
Open this link in ur browser:
http://nymag.com/thecut/2013/09/pill-no-prob-meet-the-pullout-generation.html

On the page center left you will see:

{% highlight bash %}
10.1k 
Shares

{% endhighlight %}

Amazing!
How is this working?

We have one scheduler project (Lets call it P) asked the backend API server (Lets call it A) to update view count of 1000 articles every 10 minutes.  
and A asked a CDN cache control server(Lets call it C) to purge cache for those 1000 articles every 10 minutes.   
so P now asks A to update view count of 1000 articles with param _with_/_without_ purging cache via C.
Then C sends out to CDN to purge claning the cache...

Wait...CDN, what is it?

To understand CDN, you need to understand dynamic content.

Dynamic content in CDN is not very good solution, usually need the technology and method is very troublesome to achieve these functions, such as I had designed a method of dynamic cache, based on the session bar connected, then according to the focus to control dynamic buffer time. The implementation of Cache technology mainly includes CSI, SSI, ESI, several.

In a dynamic Webpage, content update and change, but this does not mean that could not be cached, but actually there are still 90% of the content can be done in CDN. Want to spend bit of idea. But these are for customers to have more high requirements. 

Dynamic Cache pages have some scheme: 

#Client Side Includes(CSI):

　　Through the iframe, JavaScript, Ajax and other forms in addition a dynamic page content included. To achieve dynamic.

　　 Advantages: 
{% highlight bash %}
capable of parallel processing system and 
loading the browser 
client; this technique without server 
support and modification, 
calculation and operation on the client, 
can reduce server 
pressure
{% endhighlight %}

　　 Disadvantages: 
{% highlight bash %}
search engine optimization problem; 
JavaScript compatibility issues; 
the client cache may lead to 
no work timely server content updated. 
To solve the often by adding JS version
{% endhighlight %}

#Server Side Includes(SSI):

　　SSI it is a HTML file, can through the comment line called command or pointer. The content of the web site update. SSI requires the file suffix special(shtml,inc).

　　 Advantages: 
{% highlight bash %}
SSI technology is general, not specific 
language restrictions, only need a Web 
server or application 
server support, Ngnix, Apache, Tomcat, 
Jboss have good support, the Squid 
does not support.
{% endhighlight %}

　　 Disadvantages: 
{% highlight bash %}
SSI can not be directly included in the 
other servers in the grammar of URL, can 
run on the server. 
So through the CDN, Cache, or failure, 
is not flexible
{% endhighlight %}

#Edge Side Includes (ESI):

　　 Edge Side Includes(ESI) And Server Side Includes (SSI) and similar functions. SSI requires the file suffix special(shtml,inc). ESI(Edge Side Include)By using a simple markup language for those who can be accelerated and cannot be accelerated in Webpage pieces of content description, Each Webpage were divided into small portions of different with different control strategies cache respectively, The Cache server can be based on these strategies in sends complete Webpage to the user before the small part of different combination of dynamic. Through this control, can effectively reduce the number of servers from crawling the entire page, but only from the original server extracting a few not cached fragments, thus effectively reduce the load of the original server can, at the same time, improve the response time of user access.

　　Advantages: 
{% highlight bash %}
the ESI is more suitable for the cache, cache 
the entire page or page fragment, so ESI is 
particularly 
suitable for caching, CDN the first boss, 
Akamai support agreement. 
It's the most friendly for layout and Cache.
{% endhighlight %}

　　Disadvantages: 
{% highlight bash %}
come out for a long time, there has been no many 
people use. This technology programmer not. 
{% endhighlight %}


#Conclusion:

So it's up to you to choose the technology, 
but it's up to the technology to fit your project.
Becareful and keep practicing!
;P 

----------------------------------
Btw this article is for celebrating our company's new achievement in 2015!

Congratulations to everyone in NYMag team on our 10 (!) National Magazine Award nominations (which may be our personal record; not sure).  
So much deserving work honored (also so much ignored, but that's show business.)  
We had a great year, and everyone, nominated and not, deserves to feel enormous pride in our efforts — not just today, but everyday.

Here’s a link to the full list of nominees and ASME’s press release:
http://www.magazine.org/asme/national-magazine-awards-2015-finalists-announced
 
 




