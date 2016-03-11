---
layout: post
title: "Front-end composition at Trunk Club"
date: 2015-02-20 11:50
author: Josh Habdas
comments: true
headshot: https://res.cloudinary.com/trunk-club/image/upload/gazjgqce7n61r00bjftm.jpg
categories:
  - programming
  - ui
tags:
  - web apps
  - single-page apps
  - rich internet apps
  - web engineering
  - fat client architecture
  - commonjs
  - npm
  - bower
  - es6
  - modules
  - composition
---

As the Web shifts from a web of content as we’ve known it to an application platform there’s been a renewed emphasis on [composition in web app architecture](http://addyosmani.com/blog/architecture-on-the-road-to-2015/). To manage composition in our Web UIs at Trunk Club we use a number of techniques and patterns to help scale our suite of rich-clients while avoiding duplication of common componentry. This article will discuss some of the patterns we use, describe the concept of a component library and introduce software for sharing modules between apps without use of WebPack or Browserify.

> Learn how to use JS modules and a simple component library to share code in a forward-looking way.

<!-- more -->

After building [Brunch With Panache](https://github.com/trunkclub/brunch-with-panache) and using it to stand up a number of fat-client apps we started seeing duplication in our domain models and other common application components. For example, each of our front-end apps requires use of a _User_ model, a _SecureController_ and a _LoginView_. Sharing common modules in this manner has traditionally required use of machinery like Browserify or WebPack, or, depending on the circumstances, manual duplication or a clever task-runner set-up with [Grunt](http://gruntjs.com/), [Gulp](http://gulpjs.com/) or [Broccoli](https://github.com/broccolijs/broccoli).

To remove duplication from our front-end applications and to allow for shared components we created [Brunch with Coalescence](https://github.com/trunkclub/brunch-with-coalescence), a Brunch-based [Bower](http://bower.io/) component suited for storing reusable CJS, ES6 modules, and CSS for use in among apps. The primary goals are thusly:

- Limit interruption to existing Brunch app development workflows
- Avoid unnecessary architectural complexity
- Build upon current CommonJS module architecture, while planning forward for dynamic modules with ES6

Given most of our existing fat-clients use Brunch and a [custom set of Jake tasks](https://github.com/trunkclub/brunch-with-panache/tree/1.0.0/jakelib) we leveraged the existing [Jake](http://jakejs.com/) CLI (part of the [Brunch Toolchain](https://github.com/jupl/btc)) to limit interruption to the existing dev workflow — allowing common components built with _Coalescence_ to be built in a consistent and familiar way.

To avoid introducing unnecessary architectural complexity _Coalescence_ forgoes the use of [WebPack](http://webpack.github.io/) or [Browserify](http://browserify.org/) and, instead, taps directly into the consuming apps’ Brunch build pipelines using [a symlink and a Bower component](https://github.com/trunkclub/brunch-with-coalescence/wiki/How-To-integrate-with-Brunch-with-Panache)) — distributing pre-compiled source for later transpilation. The advantage of distributing precompiled and not using other tools here is less cognitive overhead, less code to manage and a streamlined, forward-looking development process.

As a result of pre-compiled distribution it’s possible to prototype changes to shared components within the apps themselves, copy those changes back to the component library and avoid working simultaneously in two repos. Additionally, existing CommonJS modules and Mocha tests are liftable, meaning they can be dropped from any _Panache_ app straight into _Coalescence _and Just Work™.

We’ve included the Babel transpiler to enable creation of ES6 modules to tee up for AMD-style dynamic module loading and protocol-level bundling with [SystemJS](https://github.com/systemjs/systemjs) and the imminent HTTP/2 protocol. For further reading on this approach, please see the *Future ES6 Loader Bundling Scenarios* section of [Practical Workflows for ES6 Modules](http://guybedford.com/practical-workflows-for-es6-modules).

The following figure is an abstract representation of how _Coalesence_ (i.e. _Core_) fits into our front-end architecture:

![Coalescence as a Core for Apps](/assets/core.png)

In the center we have a _Core_ surrounded by a number of _App_, each of which requires access to shared functionality. The _Core_ itself is a separate entity but is consumed by each _App_ — resulting in shared code. Bear in mind not everything is suitable for the _Core_. Applying Pareto’s principle, in which anything a few (20 percent) are vital and many (80 percent) are trivial, strive for _Core_ to compose approximately one-fifth of your app.

Shown to the sides of the _Core_ are examples of web components. The components may live inside _Core_, the consuming _App_ or come from a [dynamically loaded vendor library](http://techblog.trunkclub.com/avoiding-front-end-spof-in-single-page-apps/), NPM module or Bower component. Because both  likely to be used in multiple apps in multiple contexts, and are good candidates for reuse in a component library.

We’re excited for the road ahead with ES6 modules and the benefits of simplicity of design, and are looking forward to continuing to innovate while we build our way through 2015.

For additional reading on [Brunch with Panache](https://github.com/trunkclub/brunch-with-panache) please see the following previous posts and information:

- [Slides from May 14th HTML5 Meetup BWP talk at Trunk Club](https://speakerdeck.com/jhabdas/brunch-with-panache)
- [Brunch with Panache: Consistency, Simplicity, Style](http://slides.com/jhabdas/brunch-with-panache#/)
- [Brunch With Panache 0.9.0 Released](http://techblog.trunkclub.com/brunch-with-panache-0-dot-9-0-released/)
- [Brunch With Panache 0.8.4 Released](http://techblog.trunkclub.com/brunch-with-panache-0-dot-8-4-released/)
