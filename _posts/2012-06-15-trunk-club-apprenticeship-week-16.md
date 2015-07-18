---
layout: post
title: ! 'Trunk Club Apprenticeship: week 16'
author: Jean Bahnik
categories:
- apprenticeship
---
I started working on the Kissmetrics and Leviathan integration this week. It’s the first time that I have worked with our www application. Browsing the code and finding what I’m looking for takes a little bit of help but it’s definitely much less cryptic than I thought. Familiarity with our business and our code base on the API side is definitely helping.

<!-- more -->

I also started using Resque for queuing background jobs. That’s another thing that feels really straight-forward now and was difficult to implement a few months ago. We’re sending the data to Kissmetrics asynchronously to reduce the load. This is a good use case for asynchronous jobs and we’ll be looking into more jobs that could use this method and speed up our application.

I finished reading the Sinatra book that I picked up a few weeks ago. I put it down for too long after moving away from the Sinatra app that I was working on but finally finished it. I started reading the next book on my list: [Seven Languages in Seven Weeks](http://amzn.com/193435659X) by Bruce Tate. Going through the Ruby chapter was quick, a nice refresher. I’m moving on to IO this weekend.

Next week: finishing the Kissmetrics project, improving Trunkboards and getting ready for my four month review.
