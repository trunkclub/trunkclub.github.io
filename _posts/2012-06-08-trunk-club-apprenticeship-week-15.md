---
layout: post
title: ! 'Trunk Club Apprenticeship: week 15'
author: Jean Bahnik
categories:
- apprenticeship
---
This week we had our second hack day: different teams, different projects, same goal to build something in a day that could benefit the business. This time we had several themes to pick from, including Salesforce integration, social graph and inventory management. Our team built a notification system for stylists to be notified when an item that is not in stock becomes available. We also built a neat tool to search items in our inventory while building a trunk. The notification feature will soon make its way to production, which would make it the first feature coming out of hack day to make its way to our main application. The other team built an iPad app that lets you vote for your most stylish friend after pulling them from Facebook, a fun game that could help us with lead sourcing. The rest of the company will get to pick the winner next week during our demo time.

<!-- more -->

I got to play with Redis for caching. I am trying to improve the performance of Trunkboards. All of these queries add up and it took too long for the page to load in my opinion (around 20 seconds). So I started caching the data using Redis for the queries that took the longest to respond, running them if there is no data in Redis and expiring them after an hour. It’s much better, with the page load time down to around 5 seconds. But I think it can be improved more by running the queries through a background job at regular intervals and have Trunkboards only query Redis. For now, I’m waiting to get feedback from users before working on the next features of Trunkboards.

I got a new project: the Kissmetrics integration. As part of our push for data we will start using Kissmetrics to track our sales funnel. This requires me to record a variety of events throughout our applications. I’m very interested in this project because it will force me to understand our front-end application and how it interacts with our API.

Next week: Kissmetrics, Resque/Redis for queuing, finshing a book and starting a new one.
