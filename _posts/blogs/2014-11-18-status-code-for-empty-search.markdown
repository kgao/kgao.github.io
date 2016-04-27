---
layout: blog
category: blogs
title: 'Http Status Code for An Empty Search Page'
date: '2014-11-18'
author: Kgao
tags: 'SEO,200,204,400,404'
public: true
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.


## Problem:

We have a site with faceted search but sometimes when someone drills down too far it ends up with no results. 
i.e. Try search ')()(' in google, you will get:
{% highlight bash %}

Your search - )()( - did not match any documents.

Suggestions:

    Try different keywords.

{% endhighlight %}

While the page Http Status Code is still 200, instead of going to 404 page, like:

https://www.google.com/404

We actually did similar thing like google.

As a media company, we have been advised from our SEO consultant: 

A handful of the empty search result page URLs are soft 404ing tag pages,

Eg: An empty search result page from vulture.com
{% highlight bash %}

http://www.vulture.com/news/tyra-banks/4160/
http://www.vulture.com/news/fall-2009/11209/

{% endhighlight %}

These pages are returning status code 200 right now, URLs like this need to return status code 404 instead of 200.


## Challenge: 

From developer team perspective, it looks like 200 is fine, since we do serve resoponse from our backend.
From desginer team perspective, they don't really care the http status code, as long as they can have a good UI with customer infomation on it.
From SEO team perspective, they need 404 instead of 200.

So one solution might fit all needs would be a customer 404 page.

It's easy to do when you run a personal blog or a small website with less than 100 pages.
However it required more work to do when you run a high traffic website with 5 subsites and 5 domains behind the web server.
Most importantly, it might not worth to do those redirects and other hacky codes in your middle layer.

## Research:

Let's take look of the standard again:


{% highlight bash %}
200 OK

The request has succeeded. The information returned with the response is dependent on the method used 
in the request, for example:

GET an entity corresponding to the requested resource is sent in the response;

HEAD the entity-header fields corresponding to the requested resource are sent in the response without 
any message-body;

POST an entity describing or containing the result of the action;

TRACE an entity containing the request message as received by the end server. 



204 No Content

The server has fulfilled the request but does not need to return an entity-body, and might want to 
return updated metainformation. The response MAY include new or updated metainformation in the form
 of entity-headers, which if present SHOULD be associated with the requested variant.

If the client is a user agent, it SHOULD NOT change its document view from that which caused the 
request to be sent. This response is primarily intended to allow input for actions to take place 
without causing a change to the user agent's active document view, although any new or updated 
metainformation SHOULD be applied to the document currently in the user agent's active view.

The 204 response MUST NOT include a message-body, and thus is always terminated by the first 
empty line after the header fields. 



400 Bad Request

The request could not be understood by the server due to malformed syntax. The client SHOULD 
NOT repeat the request without modifications. 



404 Not Found

The server has not found anything matching the Request-URI. No indication is given of whether 
the condition is temporary or permanent. The 410 (Gone) status code SHOULD be used if the 
server knows, through some internally configurable mechanism, that an old resource is 
permanently unavailable and has no forwarding address. This status code is commonly 
used when the server does not wish to reveal exactly why the request has been refused, 
or when no other response is applicable. 


{% endhighlight %}


Also, when you feel you are trying to do something wrong or uncomfortable, here is some 
advice from google:
http://googlewebmastercentral.blogspot.com/2014/02/faceted-navigation-best-and-5-of-worst.html


## Dicussion:

(1). 204: NO CONTENT
Some one recommended 204 No Content as the request was successful. On list methods a 404 makes me think the API method does not exist.

(2). 200: SUCCESS
Some one recommended using the 200 status for empty collections, as there is a message body (an empty array in your case).
This confirms that the request was valid and successful, but says nothing about the contents of the message body (and in this context, it shouldn't!)
And recommended not using the HTTP status code 204 for this, because RFC 2616 defines the following about it:
{% highlight bash %}
	"The 204 response MUST NOT include a message-body, and thus is always terminated by 
	the first empty line after the header fields."
{% endhighlight %}

Some one also aggreed 200 with an empty list makes more sense from the point of the consumer, who would need to handle a 204 without a body differently from 200 with an empty list.  This seems confusing...  It's not as if there's no resource to return - it's an empty collection resource.

(3). 404: NOT FOUND
Some business partner and SEO team would like to suggest this, return a 404 when that happens, as no result were found thru search. I understand where he is coming from,(Google Webmaster) but still not sure if that is the correct thing to do.

(4). 400: BAD REQUEST
Seems like we should never blame users not matter what they ask, no one supports this yet, lol.

## Conclusion:

I was searching around for 200 vs 204 discussion on an empty REST collection, and am glad I dig this and find out many different thoughts. Ppl all bring out excellent points for either 200 with an empty collection or 204 with no content (I agree that 404 is a bad approach). I feel that 204 fits well (successful but no content) and seems to embrace the HTTP standard.  After noticing that Rails by default favors a 200 with an empty array for JSON responses and reading the other comments here, I see how that can be a consistent result as well.  Granted, either way, consistency in the API is important (choose 204 or 200 with an empty collection but use that pattern consistently in the API).

Browser behavior adds another interesting wrinkle to this discussion.  Navigating to a URL that returns 204 in Chrome, Firefox, and IE results in an aborted or cancelled request.  These browsers all stay on the current page while showing the new URL in the address bar.  This has confused me when using a browser to inspect a REST API.  Chrome and IE developer tools also make this look like a kind of request failure (show status cancelled in Chrome dev tools), and I have to dig into the result to see that it was actually a 204.  I don't know why browsers don't simply display a blank page like about:blank (although I'm sure there's a reason).  On the other hand, if the API returns a 200 with an empty collection, the browser will simply navigate to that URL and display the empty array.  To me, browser behavior for 204 violates the principle of least astonishment (at least, it astonished me), plus we don't suppor a customer 404 at this moment, which makes 404 more scary to users, this pushes me to keep using 200 with an empty collection.

Well, at least for now 
:P




