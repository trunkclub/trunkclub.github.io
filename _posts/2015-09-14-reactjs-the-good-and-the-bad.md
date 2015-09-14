---
layout: post
title: ! 'React.js: the Good and the Bad'
author: Jason Block
categories:
- javascript
---

There have been many [blog](http://techblog.netflix.com/2015/01/netflix-likes-react.html) [posts](http://yahooeng.tumblr.com/post/101682875656/evolving-yahoo-mail) by tech companies about how [React.js](https://facebook.github.io/react) has helped them solve a myriad of problems and make awesome interfaces. We've been using it for about six months across many of our applications and can't keep quiet about it any longer: We love it! Here's a list of reasons why it has helped us immensely, and some warts that we're still working out.

<!--more-->

## The Bad (ಥ﹏ಥ)

### Imperative programming instincts
One of the first pull-requests made to our first 100% React application was to include [jQuery](http://jquery.com/) (since the engineer wanted to call `$('.js-class').slideDown()`). The declarative nature of React makes a ton of sense when presenting a document, but our brains jump to an imperative model when we think about modeling the complex state changes of an interactive application. As a community, however, we're starting to think differently. Take Cheng Lou's [React Motion](https://github.com/chenglou/react-motion) library for example--it lets you model animations and transitions as dynamic values affected by physical constants, but in a declarative way. 

### A fair amount of wheel-reinventing
Speaking of jQuery, we didn't have the time to make our own internal version of [Foundation](http://foundation.zurb.com/), so we decided to use [Foundation](http://foundation.zurb.com/)! The downside is that many of the more advanced features of Foundation depend on javascript, and in particular they depend on jQuery. This meant that things like dropdown menus had to be rebuilt by our teams in React or sourced from other projects. Don't forget about the hundreds of billions of jQuery plugins out there that many of us have subconsciously become dependent upon.

### It's bright and shiny and everyone wants to keep painting it chrome
Flux frameworks! ES7! Generators! Babel! There are so many new and shiny things coming out of the front-end community. This is great! It means that the community is active and excited. New language specifications mean that our applications can be even more dynamic, but all of these bright new toys have caused some fatigue amongst our engineers. It hurts the confidence in our tooling choices when the community **constantly encourages and pushes these new toys as dogma**. The common joke around here is "this React pull-request looks good, but when are we going to rewrite it?” To combat this, we’ve picked a few core technologies that we’re going to stick with, even if the Music Man comes to town and starts singing the praises of New Javascript Framework X. 

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">JavaScript for Millennials via <a href="https://twitter.com/devboy_org">@devboy_org</a> <a href="http://t.co/kNNHYoVgs2">pic.twitter.com/kNNHYoVgs2</a></p>&mdash; Jesse Warden (@jesterxl) <a href="https://twitter.com/jesterxl/status/598885321808424960">May 14, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## The Good! (ﾉ◕ヮ◕)ﾉ*:･ﾟ✧

### It fit our engineering culture's programming model
There is absolutely no reason why you can't make a powerful web application with [Ember.js](http://emberjs.com/), [Angular.js](https://angularjs.org/), [Backbone.js](http://backbonejs.org/), or even regular, [framework-less javascript](http://vanilla-js.com/). But with the way we staff our team, **we needed something that appealed to the instincts of an engineer that works across the entire stack of our systems**. For that, we needed a declarative way to generate a user-interface--a “function” that took in data, produced HTML elements on the page, and afforded componentization and interactivity. It turned out that a framework with built-in MVC-like patterns was overkill for our applications. Using React lets us build reliable applications faster and **makes it incredibly straightforward for new engineers to understand what is going on**. 

### It gave us immediate satisfaction
Our [Backbone](https://backbonejs.org)/[Chaplin](http://chaplinjs.org/) applications required lots of ceremony and verbosity to get something to appear on the page, let alone have something interact with it. React removed much of that complication. There's a great beauty to your javascript applicaitions when you don't have to manually call a "render" function. React embraces this concept of displaying _data_, rather than relying heavily on stateful models to drive behavior. It treats your Javascript code like a 3d rendering pipeline more than an HTML generator. 

### It is helping us create an abstraction for common functionality
Most of our engineering work is for internal applications--warehouse management tooling, inventory management software, product catalogs, and CRM systems. We have a myriad of different applications with a number of different purposes. But nonetheless, there are a few atomic pieces that need to be shared. Long ago we attempted to do this with [bower and some symlinks](http://techblog.trunkclub.com/programming/ui/2015/02/20/front-end-composition-at-trunk-club.html).

Nowadays, we maintain a repository with shared ES6 (or ES2015/2016) components that we can pull into our applications with NPM. We don't need our systems to be paint-by-numbers, but there [atomic pieces](http://atomicdesign.bradfrost.com/chapter-2/) of interface logic that can easily be shared. And with React's component model (and a little help from Babel and Webpack), we have an ironclad contract as to the component's lifecycle in the application.


## So should you use React in your javascript applications? ¯\_ಠ ͟ʖಠ_/¯

### You already use [Webpack](https://webpack.github.io/) or something similar

If you already use a tool like [Webpack](https://webpack.github.io/) or [Browserify](http://browserify.org/) to build and compile your javascript & associated assets, adding React to your applications is very easy. We started by incrementally replacing views in our [Backbone](http:///backbonejs.org) applications (one of our front-end devs, Peter Piekarczyk, [gave a talk about that](https://speakerdeck.com/ppiekarczyk/the-hybrid-backbone-and-react-app) at JSConf 2015!). That led to the team getting more comfortable with the pattern of React components, which made it easier to start writing entire applications with it.

<script async class="speakerdeck-embed" data-id="6da6a60cd6c542c4a2072df9cd0772ed" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script> 

### Your applications don’t manage a ton of business logic

We tend to push the burden of managing business logic to our servers. This is great for client-side applications as you spend more time **shepherding XHR requests to and from servers** than computing and modifying data. As a result, applications like this are more reactive, and really fit nicely with the programming model of react components--where the UI is declaratively represented from a tree of data. 

### You’ve spent a ton of time debugging render cycles in your applications

Manually rendering views is painful, especially when the applications get more and more complicated and interactive. You...just don’t have to do that when writing React components. If something isn’t rendering, it’s because the data the component is representing is bad. You get a lot more things done correctly on the first try with React components.

===

We've had 100% React applications in production for a few months now, and we're not looking back. Do you use React in production? Let us know!

PS: Here is a list of some neat technologies we’re using in our newer applications:

- [Webpack](https://webpack.github.io/)
- [React](https://facebook.github.io/react/)
- [Object-Path](https://github.com/mariocasciaro/object-path)
- [Alt](http://alt.js.org/)
- [Babel](http://babeljs.io)

===

[Jason Block](mailto:jason@jasontheblock.com) is a front-end developer at Trunk Club, and he occasionally spews word vomit on twitter [@jasontheblock](https://www.twitter.com/jasontheblock).

ALSO: If you want to write React applications with us, [we're hiring](https://boards.greenhouse.io/trunkclub/jobs/44920)! 