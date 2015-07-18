---
layout: post
title: "Avoiding Front-End SPOF in Single-Page Apps"
date: 2014-07-08 18:24
comments: true
categories: 
  - programming
  - ui
  - performance
author: Josh Habdas
---

A couple years back Steve Souders gave a great talk at Fluent Conf titled _Your Script Just Killed My Site_ ([video](https://www.youtube.com/watch?v=aHDNmTpqi7w)). During the talk Steve explained front-end <abbr title="Single Point of Failure">SPOF</abbr> and pointed towards [a nice tool](http://blog.patrickmeenan.com/2011/10/testing-for-frontend-spof.html) for detecting it. Fast-forward a couple of years and front-end SPOF is still a concern in web development. And, when building a single-page app, SPOF is an even bigger deal as it can cause an entire web app to become unresponsive, putting users at the mercy of the browser to download and execute 3rd-party scripts prior to bootstrapping. Read on to learn how to avoid front-end SPOF using Trunk Club's single-page app skeleton, [Brunch with Panache](https://github.com/trunkclub/brunch-with-panache) (BWP).

> Learn how to avoid front-end SPOF using Trunk Club's single-page app skeleton, Brunch with Panache

<!-- more -->

Avoiding front-end SPOF in a single-page app like those created with BWP is relatively simple, but often flies in the face of what 3rd parties suggest in their implementation guides. Here's the typical site integration approach advocated by many 3rd parties, even today:

1. Insert our script into the HEAD or BODY of your main template. We should be first. Or last.
2. Don't worry, our response times are really, really fast.
3. And we're using a script-loader with, ajax so everything will be fast.

I've seen this approach advocated for a slew of products including Google Analytics, Typekit, BrightTag, Test and Target, Bazaarvoice, and it's very misleading. If you're dropping a SCRIPT tag into your template markup, and the [`DOMContentLoaded`](https://developer.mozilla.org/en-US/docs/Web/Events/DOMContentLoaded) event hasn't fired yet, you're primed for SPOF. I've seen tiny script-loaders cause upwards of 20 second page load times. There's a better way.

If you're building a single-page app the solution is simple, wait until the app begins to initialize before calling scripts, and call them all in a [non-blocking manner](http://calendar.perfplanet.com/2012/the-non-blocking-script-loader-pattern/) from the application's own JS.

To avoid SPOF in [BWP](https://github.com/trunkclub/brunch-with-panache) single-page applications, simply do the following:

- For each `script` with an `href` attribute, move the URL provided to `initialize.coffee` and load them asynchronously. If using jQuery or Zepto, this [can be done](http://davidwalsh.name/loading-scripts-jquery) using `$.getScript` and `$.get`, respectively. If you're not using either of those libraries simply build or borrow [something similar](https://gist.github.com/colingourlay/7209131) to load the files in a non-blocking fashion.
- For any `script` that runs a script-loader, embed that code into `initialize.coffee` and you're all set.

If you experience any issues with browsers that are not evergreen like Chrome and Firefox just ask the magic StackOverflow, or look into browser quirks (no, I'm not referring to IE's venerable quirks mode).

Below is a sample of the `initialize.coffee` file used in one of our major production applications at Trunk Club, showing how we load our analytics provider as well as web fonts:

``` coffeescript
'use strict'

utils = require('lib/utils')

initialize = ->

  # Add Analytics for tracking with Segment.io
  `
  window.analytics||(window.analytics=[]),window.analytics.methods=["identify","track","trackLink","trackForm","trackClick","trackSubmit","page","pageview","ab","alias","ready","group","on","once","off"],window.analytics.factory=function(a){return function(){var t=Array.prototype.slice.call(arguments);return t.unshift(a),window.analytics.push(t),window.analytics}};for(var i=0;i<window.analytics.methods.length;i++){var method=window.analytics.methods[i];window.analytics[method]=window.analytics.factory(method)}window.analytics.load=function(a){var t=document.createElement("script");t.type="text/javascript",t.async=!0,t.src=("https:"===document.location.protocol?"https://":"http://")+"d2dq2ahtl5zl1z.cloudfront.net/analytics.js/v1/"+a+"/analytics.min.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(t,n)},window.analytics.SNIPPET_VERSION="2.0.6",
  window.analytics.load(utils.app.regexes.origin.prod.test(window.location.hostname) ? "44yg1der8p" : "moz42de0rp");
  `

  # Add Typekit for web font support
  `
  (function(d) {
    var config = {
      kitId: 'led3rp',
      scriptTimeout: 3000
    },
    h=d.documentElement,t=setTimeout(function(){h.className=h.className.replace(/\bwf-loading\b/g,"")+" wf-inactive";},config.scriptTimeout),tk=d.createElement("script"),f=false,s=d.getElementsByTagName("script")[0],a;h.className+=" wf-loading";tk.src='//use.typekit.net/'+config.kitId+'.js';tk.async=true;tk.onload=tk.onreadystatechange=function(){a=this.readyState;if(f||a&&a!="complete"&&a!="loaded")return;f=true;clearTimeout(t);try{Typekit.load(config)}catch(e){}};s.parentNode.insertBefore(tk,s)
  })(document);
  `

  # Add Davy promises if available and we are using Exoskeleton
  if Backbone.Deferred and Davy?
    Backbone.Deferred = ->
      new Davy

  # Start application
  Application = require('application')
  new Application

# Initialize the application on DOM ready event.
# Use jQuery if available. Otherwise use native.
if $?
  $(document).ready(initialize)
else
  document.addEventListener('DOMContentLoaded', initialize)

```

**Tip:** Don't forget to add the `'use strict'` as shown above to enforce strict mode on the embedded JS.

And if you've [enabled CoffeeLint](https://github.com/brunch/coffeelint-brunch) (you are linting, aren't you?), you can omit linting of `initialize.coffee` by updating the [`brunch-config.js`](https://github.com/brunch/brunch/blob/master/docs/config.md) with a negative-lookahead assertion like so:

``` javascript
plugins: {
  coffeelint: {
    pattern: /^app\/(?!initialize).*\.coffee$/
  }
}
```

Questions? Just let us know.