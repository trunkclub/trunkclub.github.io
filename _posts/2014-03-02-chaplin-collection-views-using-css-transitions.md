---
layout: post
title: Chaplin Collection Views Using CSS Transitions
date: 2014-03-02 13:43
comments: true
categories:
  - programming
  - ui
  - performance
author: Josh Habdas
---

My team at work is currently porting an e-commerce SPA from an older framework over to [Brunch with Panache](https://github.com/trunkclub/brunch-with-panache) (BWP), our open source development framework for web clients. Like the old framework, BWP uses both Backbone and CoffeeScript. But to make composing applications easier BWP kicks it up a notch and adds in Chaplin, giving us Collection Views.

One of the downsides with the out-of-the-box Collection Views provided with Chaplin is that they use JavaScript-based animation to fade-in the item views once the collection has been fetched. And while this may be OK for many applications, it's not ideal for apps with pages which have many collection views, or for mobile user agents in general.

<!-- more -->

After a quick look through [Chaplin.CollectionView](http://docs.chaplinjs.org/chaplin.collection_view.html) I noticed a property called `useCssAnimation`. The default value of this property was set to `false`. Time to search GitHub for some inspiration...

A quick search for `"useCssAnimation:"` resulted in a number of hits. But [one of them](https://github.com/molefrog/steviewhale/blob/0f665a4b77daa2023db5ebf5809c3b54b50d6931/app/views/shot/grid/shotGridView.coffee) in particular caught my eye, because I recognized it was using a library I'd been experimenting with called [animate.css](https://github.com/daneden/animate.css). And I knew testing it out would be a cinch.

Given BWP is geared for developer productivity, here's all that was needed to make the switch to use *animate.css* to replace the JS-based `CollectionView` animation behavior:

1. With `jake watch:dev` running (and nginx pointing at the app's `public` directory)
2. Run a `bower install --save animate.css`
3. Open the `collection-view.coffee` and add the following two properties to the object prototype:
  
    `useCssAnimation: true # flag to enable use of CSS animations`
    `animationStartClass: "animated fadeIn" # provides CSS fade-in animation`

Test, commit and that's it! The project now uses CSS animations to replace the more sluggish and processor intensive JavaScript animations Chaplin provides by default.