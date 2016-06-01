---
layout: blog
category: blogs
title: 'EmpireJS Highlights and Thoughts'
date: '2016-5-31'
author: Kgao
tags: 'Javascript, Node.js, Conference'
public: true
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.


I went to the Empire JS (http://2016.empirejs.org) conference last week, 
and I would like to share some interesting talks here:


All you need is the desire and love of Javascript:

## ***Empire JS 2016***
    
    Time: 
    May 26 & 27.

    Location:
    The National Museum of the American Indian,
    1 Bowling Green,
    New York, NY 10004 


## ***Highlights***


### I ❤ Music & the Web	###
> Tim Pietrusky

Human ears can catch music frequency range from 20Hz to 20000Hz.Tim found a way to create and visualize music in the browser using MIDI devices.

Using Javascript + [Loop drop](https://github.com/mmckegg/loop-drop-app), he built a virtual music studio - [Nerddisco](https://github.com/NERDDISCO) 
which replaces the $$$$ DJ devices like [Roland TR-808](http://codepen.io/pixelass/full/adyLPR), MIDI Controller. 
(With surface book’s touch screen, you can turn the tablet as a real DJ controller.)


He also introduced [Web VJ Group](http://webvj.ninja/) – a working group of web devs who love making visualization using different web APIs:


The live VJ demo was pretty expressive.
i.e. [Analyze the audio](http://nerddis.co/audioanalyzer).




### Automagic UI Elements ###
> Abdella Ali

Introducing Redux: 
[Redux](https://github.com/reactjs/redux) is a predictable state container:

It helps you write applications that behave consistently, run in different environments, and react way to debug, bring new experience in develop apps.

UI data driven model: action-> reducers-> store-> view (create immutable object)

### Make art with JavaScript ###
> Sher Minn Chong

She showed how to use P5.js + Vanilla canvas API + recursion algorithm in javascript to draw the Mandelbrot set – The very famous example in Fractal, which is often called the most beautiful math. 

[Code](https://github.com/piratefsh/p5js-art)
[Demo](https://piratefsh.github.io/presentations/art-with-js/slides/index.html#/6/3)

### How To Be A Great Developer ### 	
> Edward Finkler

With 20-years web dev experience, he talks about how to be a great develop in 5 aspects:
    
    1.	Practice Empathy:
    Impact users and care feeling from the rest of team.
    Anticipate questions and confusion.
    Think weeks/months/years ahead.
    Make choice with teammates in mind.
    Do not assume people know as you know.

    2.	Practice Humility:
    Always keep learning.
    Accept own mistakes.
    You could be wrong about many things.

    3.	Always Be Learning:
    Liberal in learning and conservative in using.
    Any tech can be fit depends on needs of the projects.
    Set aside a time to learn new tech.


    4.	Avoid Tribalism:
    Refuse it.
    Build communities.
    Reserve your loyalty to people.

    5.	Better Your Community
    Make people lives better with your skill.
    Listen / understand / help / empower.

### Desktop JS ###
> Evan Morikawa

He talked about the open source project – [N1](https://github.com/nylas/N1), a desktop mail app.
Using: 

    - Nw.js
    - Node webkit
    - Atom shell
    - Electron (Support: Win 10 /Mac os x)

The benefits from desktop JS app:
    
    1. "Native" support experience: No cross browser issue.
    2.  Performance: Use web workers for multi-threads.
    3.  Data storage & offline: Use client side local storage.
    4.  Plugins & Extensibility: React Components...etc.

It’s impressive that how JS can do as a modern web technology for a desktop application.
I feel it has similar idea like Microsoft UWP, but different approach.


### This is your brain on JS ###
> Tara Lindhal

Tara is a psychologist. 
And she showed us how our brain works when we are coding.

The react experiments based on:
    
    - Language
    - Feeling
    - Feedback (Positive Info/ Warning/ Errors)

A very good example is [Elm](http://elm-lang.org/)

### Node.js Releases, how do they work? ###	
> Myles Borins	

He talked about Node.js Release strategy:
    
    1.	Different stages: Patch / Minor / Major
    2.	Alternative versioning (Even: stable / Odd: Unstable)
    3.	Github labels / Cherry-pick changes
    4.	Prevent behavior change: CI / Smoke Testing
    5.	V4.4.X LTS details

Note:
    OpenSSl is going drop support line this Oct.2016 for 0.10.45 to 0.12.14.
    Which means Meteor most likely will start to move off from node v0.10.x to v4.x+
    And once 1.4 coming out and it supports v4.x+ (Azure has native support for), we can just git push Meteor project to Azure.



### References ###

Conference theme song [here](https://twitter.com/LopezCatherine/status/735874294669004800).
Mapping JS Conferences [here](http://analyticboard.com/empireJS/).




Thank you for reading, hope this will benefit your daily work. 
Feel free to leave your comment here and share with others!
:)