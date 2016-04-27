---
layout: blog
category: blogs
title: 'An Implementation for Pixel Tracking'
date: '2015-8-20'
author: Kgao
tags: 'Node.js, Pixel, Personalization, Recommendation'
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.


## Intro:

One orientation for big data is understand and estimate user's behavior. A good use case so far is recommendation system.
Like Facebook likes, Amazon recommendations, and Apple genius. How ever, all those big data movement need data collection work
to be done first. So we have Facebook's Conversion Pixel, Amazon EC2, and Google Analytics...etc.
Internet Giants use multiple solutions in different fields trying to understand users better based on their data collecting.
Among so many tracking technologies, Pixel Tracking is increasingly popular nowadays.
Why pixel? why its so popular?
Pixel tracking often involves placing a clear graphics file, called a GIF(usually is a 1x1 size img), on a webpage. The pixel doesn’t have to be clear, any element that is on the webpage can be used to track.
The invisibility can help a gif blend seamlessly into the page and also get the information that matters most: who is clicking on what?
i.e. who is clicking through on ads and ending up on a sales page.

Thus, Pixel Tracking now is growing in the whole internet world. It's a must-have feature in your site.

## How It Works:

Basically, you need use Javascript to create a pixel embed into client side page.
It will fire an HTTP GET request (like get an 1x1 pixel img) to the server, to track the movements of a user.
then you can either use system logs or database to collect the data.

It allows the server to gather general information from the consumer’s computer’s cookies and integrate that data into the platform as part of the Web conversation. The pixels communicate with cookies on a device and pull information from those cookies such as the name of the campaign that resulted in a click-through or the date of a visit. The more basic forms of pixel tracking don’t capture sensitive personal data, and they don’t track the consumer each time an action is taken, so multiple pixels from different platforms can exist on a single page, without the sale being logged multiple times. It is used in analytics and in popular targeted-advertising tactics, such as retargeting.


Today, I’m going to show you two things:
1st. How the the pixel get created on client side.
2nd. How to design the server side api.

## Implementation:

1.How the the pixel get created on client side:

Since our project is using nodejs. So I'm gonna post some idea here in javascript:

{% highlight bash %}
function getPixel(callback) {
   var siteCookie = getCookie(); // Get your site current cookie

   // get pixel generator
   var pixel = new Pixel(); //create your own pixel cookie (cross domain is a plus)

   // async generate pixel
   pixel.get(function(pixel_id) {

     // generate pixel and add into cookie
     setCookie('pixel', pixel_id, Expire_Time);

     //callback
     callback(pixel_id);
   });
 }

 function loadScript(pixel_id) {
    // create pixel dom and append to page
    var script_to_be_added = document.createElement('script');
    script_to_be_added.async = true;
    script_to_be_added.setAttribute('src','{Your Backend API to fire pixel}?pixel_id=' + pixel_id);
    document.body.appendChild(script_to_be_added);

    //load the script here:
    getPixel(loadScript);
 };
{% endhighlight %}



2.How to design the server side api:

Imagine if you use node.js + express to create a server side api for this.
{% highlight bash %}
  //Pixel collecting
  app.get('/pixel', function (req, res) {
    saveTrackingPixel(req, function (e) {
      if (e.code == 200) {
        res.send(e, 200);
      }
      else {
        res.send(e.message, e.code);
      }
    });
  });

  //Get pixel
  app.get('/pixel/:pixelId', function (req, res) {
    getTrackingPixel(req, res, function (e) {
      if (e.code == 200) {
        res.send(e.result, 200);
      }
      else {
        res.send(e.message, e.code);
      }
    });
  });

  function saveTrackingPixel(req, callback){
    // validate data
    if(!validate(pixelData)){
      callback(400); // bad request
    }
    // persist into db (connection  / release..etc)
    pool.connection();
    pool.write(req.pixelData, function(error,success){
        if(error){
          callback(500);
        }eles if(success){
          //your own customer msg
          callback(200);
        }
    });
    pool.release();
  }

  function getTrackingPixel(req, res, callback){
    // Get data from db (connection  / release..etc)
    pool.connection();
    pool.read(req.pixel_id, function(error,success){
        if(error){
          callback(500);
        }eles if(success){
          //your own data aggregate
          callback(200);
        }
    });
    pool.release();
  }  
  {% endhighlight %}


## End:

Those above just provide a basic idea in code level for implementation a pixel tracking story. There are more works related for set up db, server, applications. Also, you might have different requirements, so this is general enough to extend different usecases.

It will be useful, because every advertising platform or website can employ tracking pixels, as the creation of the pixel is a simple matter of attaching a small image to an HTML file that can execute when the page is accessed. The aggregated data could give your sales team data to decide what should be promoted next, and what to recommend to the end users.

Beside media companies, Pixel Tracking  can be a method of tracking that facilitates an easier and more e-commerce experience for consumers, but as always, the range of practices using cookies make them just as vulnerable as other methods to exploitation.

What's more, to make it cross domain / cross site, you can invent your own way to generate the pixel, thats the core, industry standard is async generation time < 1000ms. Given this, you could identify a user from different level without losing data even in a short http conversation, and you will have the ability to collect tracking data from different platform.

Now, it's time to test it's performance on your site, enjoy!



Ref: Read more about pixel concept explaination and inspired here -> http://goo.gl/ixd6iJ
