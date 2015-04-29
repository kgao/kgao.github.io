---
layout: blog
category: blogs
title: 'An implementation for Exit Intent Technology'
date: '2015-4-29'
author: Kgao
tags: 'Exit Intent, Nodejs, Web'
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.


#Intro:

Exit Intent Technology now is growing in the whole internet world. It's a must-have feeature in your site. 

Basically, you could uses Javascipt and the web browser to track the movements of a user so a special offer or opt-in form can be displayed when they show signs of leaving your website.

Isn’t that complicated for the technology itself, but it’s more about how you use it.

Today, I’m going to show you two things:
1st The value of this technology will bring to your site. 
2nd The implementation of it.

#Design:

The general theory behind exit popups is fairly simple. Let’s imagine that a user has been browsing through your awesome site, then decides to leave without doing any of the things you want them to do (buying, registering etc). In that moment of indecision we can briefly interrupt them with a message, steering them towards a singular call to action.

This art of mis-direction is incredibly successful. Why? Well, often users get overwhelmed. And when you present them with a single option it makes their decision easier.

The call to action is the important part, the more targeted you can be the better. But, lets first consider the reasons why users might leave your site in the first place:

 {% highlight bash %}
    Users finished reading your article and they were ready to leave
    Your content was irrelevant in the first place
    They couldn’t find the product they were looking for
    Customer is price sensitive and wants to wait for a discount or coupon
    User got distracted and forgot what they were doing
    Customer was just in research phase and wasn’t ready to make the purchase
    Your product was unaffordable
    Your product doesn’t do what the customer needs
{% endhighlight %}

Now that we have an idea on why users might be leaving our site, we can use our optins to try and re-engage the customer.

Let’s look at some wildly different examples below, each of which are used for a different purpose.


#Implementation:

Basically we need to meet these requirements in our team:
Event triggers if user is closing the window, opening a new tab, toggling to a different tab, moving for the bookmarks bar or the search bar.
Event does not trigger if user is using any site navigation elements or clicking anywhere else on the page.

Since our project is using nodejs. So I'm gonna post some idea here in javascript:

{% highlight bash %}
$(document).on('mouseleave', leaveFromTop);

function leaveFromTop(e){
    if( e.clientY < 0 ) // less than 60px is close enough to the top
      alert('y u leave from the top?');
}
{% endhighlight %}

Here is a version using jQuery and ouibounce lib:
{% highlight bash %}
<div id="your-modal"><div>
<script>
  var _ouibounce = ouibounce(document.getElementById('your-modal'), {
    aggressive: true,
    sensitivity: 40,
    timer: 0,
    delay: 100,
    callback: function() {
      console.log('Exit Intent fired!');
    }
  });

  $('body').on('click', function() {
    $('#your-modal').hide();
  });

  $('#your-modal').on('click', function(e) {
    e.stopPropagation();
  });

</script>
{% endhighlight %}


#What's More:

It's your turn now, you can play with it, go create some popups like newsletter signup, member only promotion...etc.
Let the user remember it first.

Don't forget AB-test. 

And check how it grows your revenue by anywhere up to x %

;P


Ref:
More exit intent explaination and inspired here:Add https://blog.gleam.io



