---
layout: post
title: "Brunch with Panache 0.8.4 Released"
date: 2014-07-14 12:43
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
author: Josh Habdas
---

Trunk Club released today a new version of the front-end seed we use to develop our web clients. With [release 0.8.4](https://github.com/trunkclub/brunch-with-panache/tree/0.8.4) the following new features have become available for immediate use:

## What's new

- Closed [13 issues](https://github.com/trunkclub/brunch-with-panache/issues?state=closed), including 2 bugs and 11 enhancements.
- Added file fingerprinting for static assets in `index.html` to enable cache busting of primary application assets.
- Express server was added to allow for more ambitious applications. Just run `jake server:dev` after installing.
- [Swag Handlebars helpers](https://github.com/elving/swag) now available and enabled by default.
- Fixed a bug in the Jake `test:code` task. `npm test` and `test:all` now both function as expected.
- Karma test runner now auto-detects and uses available browsers for unit testing.
- Unit and site testing capabilities moved under Jake task called `test:install`, allowing for faster development installations.
- Easier-to-use environment variables as [documented in the README](https://github.com/trunkclub/brunch-with-panache/blob/0.8.4/README.md#coffeeenv).
- New Jake tasks for cleaning up locally cached Node and Bower dependencies.
- Removed the _page-view_ generator in favor of the _scaffold_ generator.
- Various application and development dependency updates.

## Upgrading

To upgrade existing BWP apps, set `brunch-with-panache` as your __upstream__, merge in `tags/0.8.4`, resolve any merge conflicts and test your application.

See the [ChangeLog](https://github.com/trunkclub/brunch-with-panache/blob/master/CHANGELOG.md) for more details.
