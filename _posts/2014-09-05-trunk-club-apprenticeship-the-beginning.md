---
layout: post
title: "Trunk Club Apprenticeship: The Beginning"
date: 2014-09-05 12:01
comments: true
categories:
  - apprenticeship
author: Amelia Padua
---

The first week of Trunk Club was a bit of a blur. There was a lot to take in. There were many more people and the office space was much bigger than I expected. On top of a new environment, there were a lot of new technologies and a completely different architecture than I had seen before. My head was spinning.

The day I graduated Dev Bootcamp, I felt… exhausted more than anything (I spent a full week catching up on sleep), but I felt like I could conquer anything. When I started getting to know the developers at Trunk Club and actually digging through the code, that feeling was a bit shaken. It can be a little mind melting to think about how much there is to learn. Plus, it was hard not to be impressed by how much everyone seemed to know. It was wizardry and magic everywhere! That’s why I got into this industry, though. I love getting to learn what’s really happening under all that magic.

My job for the first week was to get situated and start getting things set up on my laptop. That was not a small task.  I didn’t realized how much went on behind the scenes to make the entire business run. I think what surprised me the most was that they don’t work on just one monolithic application. They have been in the process of moving to a [microservice architecture](http://martinfowler.com/articles/microservices.html), which means their software is broken up into several separate pieces that work together. I was going to have to learn how to get the pieces I needed onto my local environment (my computer) and get them talking to each other.

<!-- more -->

Like many startups, they use GitHub as their collaboration tool. For those who have never used [GitHub](https://github.com/about) or know what [git](http://git-scm.com/doc) is, git is a source code management system. The great thing about it is that if you commit (save to this system) the changes you make, you will have a history of your changes and if anything goes wrong, you can rollback your changes. GitHub is a site for you to store your code and save your git history (a history of any changes to the code) so that others can see and make changes to the code as well.

With over 100 repositories, essentially folders on GitHub that hold a project, the first thing to do was to figure which ones to pull and get running locally. I was added as a member of Trunk Club on GitHub and began the week of downloads. I started pretty confidently, but like most things in software, it always takes longer than you think. So. many. errors. It took me a couple of days to work through all the random issues. Some of it was things like working with FactoryGirl (used to create scenarios for testing code),
```FactoryGirl.create(:employee, :email => "username@trunkclub.com", :role => "Technology")```.
I had “technology” instead of “Technology”. I learned that you can only use either [RVM](http://rvm.io/) or [rbenv](https://github.com/sstephenson/rbenv) to manage your rubies and if you want to switch, you have to completely remove the other manager. I even learned a little bit about how the different apps and APIs talk to each other. Hello, [nginx](http://nginx.com/) (which can be a whole other blog post on its own).

I think the most important habit I decided to be religious about was taking a notebook with me everywhere I went. It can be surprisingly difficult to keep track of everything you have worked on in a day. One of the best things I was given on my first day at work was a lovely leather bound journal. At first, I couldn’t think of what I would use it for. Then I went to my first stand up.

If you’ve never attended a stand up before, it’s just a short meeting that should last less than 15 minutes in which everyone on the team says what they worked on yesterday, what they will work on today, and if they have any blockers. Typically they’re held on a daily basis. It is a great way for everyone on a team to know what other people are working on, ask for help, and product managers find out relatively quickly when there is an issue that might affect the project timeline.

Stand ups are where you need to remember what you’ve done. Once I started writing down the things I was working on, I was not only able to give a better description of what I was actually doing there, but I started to actually retain information (or at least some of it). If you’re just starting out, I highly recommend taking notes either as you go through the day. If you happen to be the type that never took notes in school because your memory is just that awesome, I suggest documenting issues you’ve run into that could benefit the next new person. It can be hard to find time to spend on documentation, but it’s one of the most useful tools. Of course as soon as I write that, I asked our engineering intern about how amazingly useful note taking is, expecting him to validate everything I just wrote, and he tells me he never writes anything down. Thanks, Ansh. Well, everyone has their own methods. Just don’t be afraid to keep a notebook of commands you might forget, context around something you will be working on, or very importantly, things you commit to doing.

My first week at Trunk Club was a really exciting week. There is a lot to learn, but I am surrounded by incredibly smart people, each as nice and as willing to help as the next, and  feel lucky to be at a company filled with people that seem to genuinely like their jobs. How do you not love a place that treats you during the day?

![Gelato!](/assets/jDd2VP6l.jpg)

To really kick things off, we had a Tech Team outing at the end of the week. Behold, bubble soccer! Not as easy at it looks, but every bit as amazing.

![bubble soccer](/assets/kSEBeoKl.jpg)

If this is any indicator of how my apprenticeship is going to go, I have a lot to look forward to!
