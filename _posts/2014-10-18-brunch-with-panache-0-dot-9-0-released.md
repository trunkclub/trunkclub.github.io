---
layout: post
title: "Brunch with Panache 0.9.0 Released"
date: 2014-10-18 18:08
comments: true
categories: 
  - programming
  - ui
tags:
  - web apps
  - single-page apps
  - rich internet apps
  - web engineering
  - fat client architecture
  - node
author: Josh Habdas
---

Trunk Club released last week a new version of the front-end seed we use to develop our rich web clients. [Brunch with Panache v0.9.0](https://github.com/trunkclub/brunch-with-panache/tree/0.9.0) is the first release to enable full-stack JS development using Node.js and [Hapi](http://hapijs.com/), making it even easier to build the single-page app you always wanted. Creating [reverse proxies](https://github.com/jhabdas/hopstop/blob/ratchet/server/index.coffee#L9-L19) is simple, and there are [plenty of plug-ins](http://hapijs.com/plugins) for use in scaling. Deploying with [Docker](https://www.docker.com/)? Check out the new Docker integration. See below for details.

## Features
In plain English (mostly).

- Improved page load speed. Perceived page load time with Elastic Search is about 250ms.
- Autoprefixes CSS for faster Sass development without [Bourbon](http://bourbon.io/).
- Run experiments faster than ever with the new Scaffold generator and other web component enhancements.
- Now capable of running as a daemon in Docker using [bwp-docker](https://github.com/trunkclub/bwp-docker).
- Build more ambitious, full-stack JS web applications.
- Better automated testing facilities for easier CI integration and confident application deployments.

<!-- more -->

## What's new [since 0.8.4](http://techblog.trunkclub.com/brunch-with-panache-0-dot-8-4-released/)
Give me the deets.

- Closed [4 issues](https://github.com/trunkclub/brunch-with-panache/issues?state=closed), and made a number of other small bugfixes and enhancements. ([view diff](https://github.com/trunkclub/brunch-with-panache/compare/trunkclub:0.8.4...0.9.0))
- Removed dependency on Ruby by favoring a C-based version of Sass called [libasss](https://github.com/sass/libsass). BWP no longer relies on Ruby.
- Provides a Hapi server. If it's [good enough for Black Friday](http://thechangelog.com/116/), it's good enough for us.
- Improved Scaffold generator to allows new pages to be created in a single [Jake](http://jakejs.com/) task: `jake g scaffold=user`.
- Fixes a bug in the `server:dev` and `server:prod` Jake tasks.
- `npm start` now compiles the app for `--production` and runs with Hapi while watching for changes to app source.
- Provides out of the box support for [Swag](https://github.com/elving/swag) for use with [Handlebars](handlebarsjs.com) and can be removed with `jake rem:swag`.
- Some issues with the [Karma](http://karma-runner.github.io/0.12/index.html) test runner have been fixed up, so `jake test:all` works like a charm.
- Browser Detect added. Now unit tests run not just thru [PhantomJS](http://phantomjs.org/), but all system-detected browsers.
- Fixed a deep-dependency issue causing source maps debugging to go haywire on OS X when `ulimit -n 10000`.
- Use [bwp-docker](https://github.com/trunkclub/bwp-docker) to start a running instance of BWP with Docker in seconds.

## Upgrading

To upgrade existing BWP apps from `0.8.4` to `0.9.0`, set `brunch-with-panache` as your __upstream__, merge in `tags/0.9.0`, resolve any merge conflicts and test your application. Once the new upstream is merged, run the following commands to update your `npm` dependencies and restart the server like so:

```
$ jake npm:clean # rm -rf node_modules && npm cache clean
$ npm install
$ jake server:dev # runs Hapi server and watches for changes (with Source Maps)
```

Please see the [ChangeLog](https://github.com/trunkclub/brunch-with-panache/blob/master/CHANGELOG.md) for more details.

## A closer look at generators
BWP is designed to minimize redundant tasks. Generators help immensely in that regard, saving time. Which is why we're proud BWP comes with a collection of built-in generators. To see what's available simply run `jake generate` (aliased with `jake gen` and `jake g`) at the terminal from within the project directory for a listing of what's possible.

As of this version, BWP supports the following generators:

```
$ jake gen
List of available generators in ./generators:
 * code-test (Test file for code testing)
 * collection (Chaplin collection and unit test)
 * collection-view (Chaplin collection view and unit test)
 * controller (Chaplin controller and unit test)
 * model (Chaplin model and unit test)
 * scaffold (Chaplin Collection View, Controller, Actions and Routes)
 * site-test (Test file for site testing)
 * view (Chaplin view and unit test)
```

A powerful generator is the scaffold generator, which combines together other generators to form a simple package for quickly prototyping new concepts. Here's what it'll do:

- Takes a dasherized-string-literal (singular form) as an input name creates files
- Creates a Model and a Collection (inflection is automatic) using the name provided
- Creates a named View and a Collection View, and their respective Handlebars template files
- Stubs out unit test files for the above
- Creates a new controller and connects everything together using a `index` and a `show` action
- Updates `routes.coffee`, enabling instant available navigation to the new page

Here's the sample output from the Scaffold generator, which takes about a second to run. These files would otherwise be created by hand, leading to less consistency and wasted effort.

```
$ jake g scaffold=user
29 Oct 00:26:00 - info: create app/models/user.coffee
29 Oct 00:26:00 - info: init test/code/models
29 Oct 00:26:00 - info: init app/views/user/templates
29 Oct 00:26:00 - info: create app/models/users.coffee
29 Oct 00:26:00 - info: init test/code/models
29 Oct 00:26:00 - info: init app/views/user/templates
29 Oct 00:26:00 - info: init app/views/user
29 Oct 00:26:00 - info: init app/views/user/templates
29 Oct 00:26:00 - info: create test/code/views/user.coffee
29 Oct 00:26:00 - info: init app/views/user
29 Oct 00:26:00 - info: init app/views/user
29 Oct 00:26:00 - info: create test/code/models/user-test.coffee
29 Oct 00:26:00 - info: create app/views/user/user-view.coffee
29 Oct 00:26:00 - info: create app/views/user/templates/user-item.hbs
29 Oct 00:26:00 - info: create test/code/views/user-test.coffee
29 Oct 00:26:00 - info: appending to app/routes.coffee
29 Oct 00:26:00 - info: create app/controllers/user-controller.coffee
29 Oct 00:26:00 - info: create test/code/models/users-test.coffee
29 Oct 00:26:00 - info: create app/views/user/user-collection-view.coffee
29 Oct 00:26:00 - info: create app/views/user/user-item-view.coffee
29 Oct 00:26:00 - info: create app/views/user/templates/user.hbs
29 Oct 00:26:00 - info: create app/views/user/templates/user-collection.hbs
```

When you need to be more surgical about the way you work, the individual scaffolding tools will cover a majority of your needs. Generators can easily be customized to create widget factories, making it easy to implement commonly-used components.