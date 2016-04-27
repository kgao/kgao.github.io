---
layout: blog
category: blogs
title: 'The Scoring and Ranking System behind Game and Quiz'
date: '2015-10-26'
author: Kgao
tags: 'MongoDB, Restful, Back End, User Interaction'
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.


## Background:

One goal for modern media today is to increase readers' loyalty.  Although the loyalty programs technology is a very big topic, including many aspects in different field. Most popular ones are: link to related posts, promote popular posts, subscribe email list, membership offers, content likes, comments, follow and share, reader interviews, direct publish on multi-media, entertainment elements(music,video,games,vote,quiz).

Today, let's focus on one piece of it, game and quiz. And the scene behind it - Scoring and Ranking System.
We released our [Voting Quiz](http://nymag.com/thecut/2015/10/sex-habits-784-college-students.html) with percentage feature last week, and last friday, we released our first [Weekly Culture Quiz](http://www.vulture.com/2015/10/vultures-week-in-culture-quiz.html) with membership rank and leaderboard feature on our great brand [Vulture](http://www.vulture.com).
fdafdfadfadsf
Vulture, the entertainment destination from the team behind New York magazine, is a beacon for passionate fans who want a smart, comprehensive take on the world of culture and offers around-the-clock, wall-to-wall coverage of movies, TV, music and beyond. Vulture's writers and editors celebrate culture both high and low, because you never know where the next truly brilliant moment will come from.

## The system design:

Basically, its a fullStack environment to achieve this. Here I wanna focus on more what I've done, so let's assume you have an awesome front end support which the UI creative works are already done.

Here are the bottom-up pictures from our back end:

1. A score database with well designed schema
   Here we use MongoDB, since I have sort of talked about why in previous articles, here I just wanna share one thought, MongoDB doc is schemaless, which a good selling point for it to give you more flexibility in design and usability, however, this doesn't means you can skip this step, you still need to think ahead before you use it, otherwise you might end up with manually 'joins' b/w 2 collections in Mongo, this will be a big headache since Mongo doesn't support join. So here you need a bit de-normalization in your data schema design. Think about which is common and unique, how to index before you `db.createCollection()`.

2. Business logic layer to meet the requirement
   You might end up with complex algorithms or join hells in your database(if step 1 failed), since we are using MongoDB here, more or less, you need think how fast you can aggregate the data to get your result.

3. A CRUD API to let front end submit score and get result.
   More specifically in our stack: we need a Restful API on top of MongoDB.
   i.e.
   {% highlight bash %}
   POST: /account/vote/quiz
   GET: /account/vote/quiz?uid={UserID}&qid={QuizID}
   POST: /account/score/game
   GET: /account/score/rank?gids={Gameids}&handle={UserName}&limit={LimitNumber}
   {% endhighlight %}

## Implementation:

1. Here is a doc example in MongoDB:
  {% highlight bash %}
  {
    _id : ObjectId("56291c3d328d58d0dddd09f1"),   //Mongo doc id, unique, automatic
    uid : "oeu1442519348199r0.06604118854738772",  //User id, using nyma(ref: pixel tracking), from user cookie, unique, not null.
    qid : "quiz-test-10-27-2015", //Quiz or game or vote id, not null
    handle : "kevin",  //User name, only for login users, nullable
    point : 995,  //score point for each quiz/game not null
    votes: [   //votes for voting quiz, nullable
        {
            "name" : "Spiderman",
            "weighted" : 1
        },
        {   
            "name" : "Iron Man",
            "weighted" : 2
        }
    ],
    level: 1  //level for voting quiz, nullable
  }
  {% endhighlight %}

  Note that this doc is simplified since quiz might have different levels, each level might have different weight...etc.

2. In buisness layer, implemented in javascript(nodejs + mongo):

  For voting quiz business logic is to get the votes ranking and percentage based on the weight of each vote in each level of a quiz:
  {% highlight bash %}

  var MongoClient = require('mongodb').MongoClient;  //import mongodb nodejs module
  ...
  //To get vote rank:
  ...
  mongoPool.collection('votes').aggregate([{  
             '$match': {
               'qid': quizId,
               'level': level
             }
           }, {
               '$unwind': '$votes'
             }, {
               '$group': {
                 '_id': '$votes.name',
                 'count': {
                   $sum: '$votes.weighted'
                 }
               }
             }, {
               '$sort': {
                 'count': -1
               }
             }], function (err, result) {
               ...
               //deal with result here: log errors, assemble json, handle response status code, callback.
               ...
             });

  {% endhighlight %}

  For game score user ranking and leaderboard:
  {% highlight bash %}

    // non-nymag users
      ...
      mongoPool.collection('scores').aggregate([
        { '$match': { 'handle': { '$exists': 1 }, 'gid': { '$in': scoreObject.gids } } },
        { '$group': { '_id': '$handle', 'points': { '$sum': '$point' } } },
        { '$sort': { 'points': -1 } },
        { '$limit': scoreObject.limit }
      ], function (err, doc) {
        ...
        //deal with result here: log errors, assemble json, handle response status code, callback.
        ...
      });

   // nymag registered users
      ...
      mongoPool.collection('scores').aggregate([
        { '$match': { 'handle': { '$exists': 1 }, 'gid': { '$in': scoreObject.gids } } },
        { '$group': { '_id': '$handle', 'points': { '$sum': '$point' } } },
        { '$sort': { 'points': -1 } }
      ], function (err, doc) {
        ...
        // apply the user own ranking logic with optimization:
        ...
        var position = 1; //record current position in loop
        var find = -1;    //record user position
        var point = -1;   //record previous point in loop
        var flg = true;   //loop breaker
        for (var i = 0; i < doc.length && flg; i++) {
          if (doc[i].points != point) { //Only increase rank position if there a higher score.
            position = i + 1;
            point = doc[i].points;
          }
          if (doc[i]._id === scoreObject.handle) {
            find = position;
          }
          if (find != -1 && i >= scoreObject.limit) {
            flg = false;
          }
          doc[i].position = position;
        }
        result.rankings = doc.slice(0, scoreObject.limit);
        result.userRanking = {
          'userName': scoreObject.handle,
          'position': find
        }
        ...
        //deal with result here: log errors, assemble json, handle response status code, callback.
        ...
      });

  {% endhighlight %}



3. In the end the endpints you actually provided:
  Submit Quiz Votes
  POST: /account/vote/quiz
  Required: UserID, QuizID, Vote, Weighted, Level, tag Response: HTTP status code
  {% highlight bash %}
  {
    "uid": "oeu1395695396875r0.4988085387740284",
    "qid": "8h4fscx",
    "votes": [
        {
            "name": "BirdMan",
            "weighted": 1
        },
        {
            "name": "IronMan",
            "weighted": 2
        },
        {
            "name": "Rio",
            "weighted": 3
        }
    ],
    "level": 1,
    "tag": "movie"
  }

  Response:
  {500: Server Error}
  {422: Invalid request}
  {200: Success}
  {% endhighlight %}

  GET: /account/vote/quiz?uid={UserID}&qid={QuizID}
  Required: UserID, QuizID Response: User Picked voteds and top rankings with percentage for each level
  {% highlight bash %}
  {
      "uid": "oeu1395695396875r0.4988085387740284",
      "qid": "8h4fscx",
      "votes": [
          {
              "level": 1,
              "vote": [
                  "BirdMan",
                  "IronMan",
                  "Rio"
              ]
          },
          ...
      ],
      "rankings": [
          {
              "level": 1,
              "ranking": [
                  {
                      "name": "BirdMan",
                      "percentage": 90
                  },
                  {
                      "name": "IronMan",
                      "percentage": 80
                  },
                  {
                      "name": "Rio",
                      "percentage": 75
                  }
              ]
          },
          ...
      ],
      "total": 999
  }

  Response:
  {500: Server Error}
  {422: Invalid request}
  {404: Not Found}
  {
      200: success,
      result: result Object
  }
  {% endhighlight %}

  Game Score Submit and Percentage
  POST: /account/score/game
  Required: uid,gid Optional: handle,score,point Response: Top Rank percentage vs all players
  {% highlight bash %}
  Post body:
    {
        "uid": "kgao",(required: nymag, for all users)
        "gid": "8h4fscx",
        "handle": "kevin.gao", (optional: username, only for longin users)
        "score" :{ (optional: compatible with old game/quiz)
            "correct" : 9,
            "total" : 10
        },
        "point" : 550 (optional: compatible with new game/quiz)
    }

    Response:
    {500: Server Error}
    {422: Invalid request}
    {
        "uid": "kgao",
        "gid": "8h4fscx",
        "handle": "kevin.gao", (optional: username, only for longin users)
        "rank": 71.42857142857143,
        "avg": 8.525606469002696
    }
  {% endhighlight %}

  Game Score Rankings
  GET: /account/score/rank?gids={gameIds}&handle={username}&limit={limitnumber}
  Required: gids Optional: handle, limit Response: Game score rankings for nymag users by weekly, monthly.
  {% highlight bash %}
  Response:
    {404: Not Found}
    {422: Invalid request}
    {500: Server Error}
    {
        "code": "200",
            "rankings": [
            {
                "id": "herve",
                "position": 1,
                "points": 2700
            },
                    {
                "id": "kevin",
                "position": 1,
                "points": 2700
            },
                    {
                "id": "sam",
                "position": 3,
                "points": 2000
            },
                    {
                "id": "scott",
                "position": 4,
                "points": 1900
            },
        ],
        "userRanking": {
              "userName": "kevin",
              "position": 1
        }
    }
  {% endhighlight %}




## End:

"One way or another, if human evolution is to go on, we shall have to learn to enjoy life more thoroughly."        - Mihaly Csikszentmihalyi.

Here I just provide a basic idea in code level for implementation a game/quiz scoring and ranking system, part of the user interaction story, also provide user more entertainment and gain more loyalty . There are more works related for set up integration test environment etc.

It will benefit us in a long run for our brand and products, to let user come back to our sites more often and more loyal to our content. Also, there is a lot of fun for doing this and completing with other NYMag readers.

So far we got some good feedback from user experience from our sites, like some of them picked from comments:

"Well, makes me feel less intimidated about being a virgin in 20s"
said by neuroticknight who did the vulture quiz.
1 day ago

"That was fun. I chunked it, but it was fun."
said by SwankyOne who did the vulture quiz.
3 days ago

"This is the most famous I've ever been."
said by allison.libert who ranked currently No.6 in our vulture weekly quiz leaderboard.
3 days ago


What's more, to make it more useful, if we got good experiences for maintain this nodejs module, we could possiblely opensource this module and express the success to the media community.

Now, it's time to for us to save the real world together!
Like Jane Mcgonigal said in book [《Reality is broken》](https://play.google.com/store/books/details?id=yiOtN_kDJZgC&source=productsearch&utm_source=HA_Desktop_US&utm_medium=SEM&utm_campaign=PLA&pcampaignid=MKTAD0930BO1&gl=US&gclid=CPHFvreUwMkCFRWlNwodXZwB_w&gclsrc=ds), lets play the game and change the world.


Enjoy:

[Voting Quiz](http://nymag.com/thecut/2015/10/sex-habits-784-college-students.html)

[Weekly Culture Quiz](http://www.vulture.com/2015/10/vultures-week-in-culture-quiz.html)
