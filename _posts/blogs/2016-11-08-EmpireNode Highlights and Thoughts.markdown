---
layout: blog
category: blogs
title: 'EmpireNode Highlights and Thoughts'
date: '2016-11-08'
author: Kgao
tags: 'Javascript, Node.js, Conference'
public: true
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.


I went to the [Empire Node](http://http://empirenode.org/) conference this Monday, 
and I would like to share some interesting talks here:


All you need is a little desire and love of Node:

## ***Empire JS 2016***
    
    Time: 
    November 7.

    Location:
    The National Museum of the American Indian,
    1 Bowling Green,
    New York, NY 10004 


## ***Highlights***


### Portable Distributed Systems – Using Node On 'Routers'	###
> Nick Hehr


Modern web application development often focuses on Real-time.
Real-time depends on 2 factors: Bandwidth and Latency. 

Bandwidth depends on infrastructure.
Latency depends on 5 factors: propagation, transmission, processing and queuing.

The goal for reduce latency is let packages hop between each Routers within 20ms, longer latency might lead into time out.

Then he introduced [Tessel](https://tessel.io/)(an IoT and robotics development platform) and a routers demo built by node.js.

Talk also covered node.js on de-centralize system, as well as
[PounchDb](https://pouchdb.com/) (an open-source JavaScript database inspired by that is designed to run well within the browser) and [Daplie](https://daplie.com/) (a personal cloud home server)






### Node Microservices Using Hydra ###
> Carlos Justiniano 

An introduction for [Hydra](https://github.com/flywheelsports/fwsp-hydra) – A light-weight microservices library for node.js he built.
 
He mentioned when people talked about microservices, they often focus on 'micro' - the size of the project, however, what we should focus on more is the 'services' - what problem did the app resolve.

The features of using Hydra: maintainable, reliable, easy to build, easy to deploy.

Build a microservice in under 15seconds - [Demo](https://twitter.com/cjus/status/796125752278351872)



### Build your child their very own node.js Forzen Robot  ###
> Gabrielle Crevecoeur

Gabrielle is a Microsoft technical evangelist from Chicago

She showed a demo that a talking robot: [ForzenNodeBot](http://aka.ms/frozenbot) could sing 'Let it go' song triggered by key words during conversation. 

Robot hardware: Ardurino(or Rasberry PI)  + Shield + Speaker.

Robot Software: [Johnny 5](http://johnny-five.io/) (A JavaScript Robotics & IoT Platform) + [Microsoft Cognitive API](https://www.microsoft.com/cognitive-services)


### Real-time machine learning with Node.js ### 	
> Philipp Burckhardt

It’s a new way to study machine learning via node.js.

Talk covered learning patterns, batch/incremental learning, prediction.
And mainly focused on 3 types of problems in machine learning: Regression, classification, clustering. (All Supervised)

In terms of real-time data visualization, he explained what’s ‘moving’ and ‘window size’.

Demo: How to predict NPM release version and React usage.

Sildes are [here](http://machine-learning.surge.sh).


### Getting started with the Microsoft Bot Framework ###
> Sarah Sexton

Sarah is a Microsoft technical evangelist from Chicago.

She showed how to get start using Microsoft Bot Framework, and the benefit using that you could publish your bot to multiple platforms. (Skype, slack, twillo…etc)

[Demo](http://tracerybot.azurewebsites.net/) , powered by tracery.io (A tool and language to generate text)

More content from [Sarah](http://aka.ms/sarahsexton)

Btw, here is my [bot](https://github.com/kgao/ok-bot), serving msr nyc’s slack channel.



### Rocketing the node community beyond the edge ###
> Tierney Coren

He talked about the 'OOS' – OPEN open source.

Reason to the Node.js popularity:
    
    1.	Apps are being published endless in community.
    2.	Easy to pick up and learn.
    3.	JavaScript.

Some statistics about node.js community:
    
    350,000 packages total published.
    500 new packages upload per day.
    6 billion packages total download.




### References ###
Empirenode [twitter](https://twitter.com/empirenode)


Thank you for reading, hope this will benefit your daily work. 
Feel free to leave your comment here and share with others!
:)