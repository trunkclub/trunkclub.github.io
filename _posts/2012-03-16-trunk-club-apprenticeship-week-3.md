---
layout: post
title: ! 'Trunk Club Apprenticeship: week 3'
author: Jean Bahnik
categories:
- apprenticeship
---
I just got a new responsibility that will help learn about our code base and our business faster: doing triage for the tech team’s ticket system. The requests are only internal and tech-related. Some of them are hardware issues but a lot of them are bugs or UI and data issues. This reminds me of my first internship in the US where I had to man the phones even though my English was not great: you have to learn very quickly… I tried to resolved as many tickets as I can and pass the rest to other people on the tech team.

<!-- more -->

We started discussing the main project that I will be working on, data visualization. having been on the management side of a business before I can certainly understand the need for more in-depth reporting of key metrics. We used to get a daily email with KPIs every morning and that’s definitely something that I would like to implement here as part of this project. It’s also the first step towards implementing a robust BI solution and a very interesting project for me. Next step will be to gather requirements from the various departments.

I learned a lot about SQL this week. ActiveRecord is great but I do enjoy building queries in raw SQL. We use Navicat to query our Postgresql databases and my mentor Mike Cruz is helping me a lot. I’m definitely getting better at querying data for other departments but still need a lot of practice.

I also got to write my first scheduled taks, using Procfile, the Clock gem and Resque. It’s cleaning up the database so that I don’t have to do it manually every day.

Next week: A lot of discussions with stakeholders to gather requirements for metrics dashboards.
