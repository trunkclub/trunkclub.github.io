---
layout: post
published: true
title: Moving from Tumblr to Octopress
comments: true
author: Josh Habdas
tags: 
  - octopress
  - jekyll
  - blogging
  - tumblr
  - prose.io
  - "travis-ci"
---

At Trunk Club, we love technology. It's at the core of our business and helps enable us to provide amazing user experiences both internally and externally. So, when asked recently to help provide opinion on our tech blog, I was excited for opportunity--until I saw the blog was hosted on Tumblr.

What's wrong with Tumblr? Nothing, really, except that, as an engineer who's been maintaining a technical blog for the last 5 years, I found it difficult to voice an opinion on a platform I myself didn't fully embrace--to me it was just another CMS, and a bit restrictive at that. So before writing this post, I felt compelled to move our tech blog away from Tumblr. Read on to learn about the Octopress migration, and how to set up a wicked quick, CMS-free blog with free hosting and then some.

<!-- more -->

## CMS blogs don't feel quite as fast

Based on personal experiences with CMS-driven blogs like [WordPress and Drupal](http://www.habdas.org/drupal-7-for-wordpress-admins/), going CMS-free was the first requirement. This was primarily a performance play, as CMS-driven sites, even Wordpress with W3 Total Cache, CloudFront and any number of performance tweaks simply can't compete with a static site witout a jauggernaut of an app server behind it. There was no second requirement. (Assume the obvious.)

At Trunk Club we write a lot of Ruby and CoffeeScript. So it seemed fitting we should chose a blog which leverages one of those technologies. After looking briefly at Jekyll, I noticed Octopress. And after looking at Octopress, I noticed it could be hosted on GitHub pages--for free--and be build by Travis-CI (also for free). That's a  lot of free, so let's briefly recap what we're building:

- Open source site to help illustrate how Trunk Club uses [Octopress](http://octopress.org/)
- Blog is of the CMS-free variety and based on the wonderful [Ruby language](https://www.ruby-lang.org/)
- Hosting righ tnow is free thanks to [GitHub Pages](http://pages.github.com/)
- Continuous Integration using [Travis-CI](https://travis-ci.org/)
- Support for [Prose.io](http://prose.io/), to simplify post authoring and enable blogging from a desktop or mobile web browser
- Integration with the 37signals' Campfire to post build notifications

## Found just what I was looking for

After some searching I was able to unearth enough material to lead me to reverse engineer what was being done, resulting in a [system diagram](http://www.gliffy.com/go/publish/4845414) I could use to illustrate how the whole thing worked. Feedback on the whole concept was positive, so I started building. After a bit of trial and error given different approaches, I ended up finding the following excellent article, which boils most of the setup process into a small number of easily reproducable steps:

[Octopress+Prose+Github+Travis CI = Coders' Blog](http://rogerz.github.io/blog/2013/02/21/prose-io-github-travis-ci/)

## Adjustments to the above to make it all work

The above instructions were less than a year old, but alone would not work for me. Please note the additional steps needed for me to get [the whole thing](http://www.gliffy.com/go/publish/4845414) working properly.

### Enabling online post authoring, web and mobile web
The [Prose configuration](https://github.com/prose/prose/wiki/Prose-Configuration) had to be modified for allow for post authoring. Here's at an early Trunk Club configuration:

```
# prose.io settings
# https://github.com/prose/prose/wiki/Prose-Configuration
prose:
  rooturl: 'source/_posts'
  siteurl: 'http://techblog.trunkclub.com'
  media: 'assets'
  metadata:
    source/_posts:
      - name: "layout"
        field:
          element: "hidden"
          value: "post"
      - name: "title"
        field:
          element: "text"
          label: "Title"
          value: ""
          type: "text"
      - name: "author"
        field:
          element: "text"
          label: "Author"
          value: ""
          placeholder: "Trunk Botterson"
          type: "text"
      - name: "categories"
        field:
          element: "multiselect"
          label: "Post Category"
          placeholder: "Create or choose"
          alterable: true
      - name: "comments"
        field:
          element: checkbox
          label: "Comments"
          value: "true"
      - name: "tags"
        field:
          element: "multiselect"
          label: "Add Tags"
          placeholder: "Choose Tags"
          alterable: true
```

### Who's that Travis fellow anyway

After modifying the `Rakefile` and `.travis.yml`, and getting the [Travis integration](https://travis-ci.org/trunkclub/trunkclub.github.io) ready, Travis was reporting successful builds although the `rake deploy` task was failing. I did some digging and was able to turn up a [nice fix](https://github.com/travis-ci/travis-cookbooks/issues/159#issuecomment-21675873), copied here for convenience:

    before_install:
      - git config --global user.name "Travis CI"
      - git config --global user.email "user@example.com"
      
### Twitter loading, loading, bueller

With a `twitter_user` set using the Octopress Unless patched, users with a vanilla Octopress install will notice *tweets won't load*, the result of an API [Twitter chose to sunset](https://dev.twitter.com/blog/api-housekeeping). Fixing this is as simple as creating a new aside, like the following file added to `_includes/custom/asides`:

``` html twitter_widget.html
<section>
  <h1>Latest Tweets</h1>
  <a class="twitter-timeline" href="https://twitter.com/jhabdas" data-widget-id="382004356658130944">Tweets by @jhabdas</a>
  <script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
</section>
```

When updating asides, don't forget to adjust the asides configuration in `source/_config.yml`.

### Lazy-loading YouTube videos

If migrating from a blog with video content hosted on YouTube, check out the [`jekyll-youtube-lazyloading`](https://github.com/erossignon/jekyll-youtube-lazyloading/) plug-in, which adds a preview screenshot image to a page and waits for a click event before appending the video `iframe` to the DOM, resulting in a snappier UI and faster download for pages with YouTube-hosted video content.

Once the plug-in is installed, YouTube videos can be embedded using the `youtube` liquid tag, así:


```
{% raw %}{% youtube f7AU2Ozu8eo %}{% endraw %}
```

## So long, Tumblbeasts

Wrapping things up, I used the [Jekyll import tool](https://github.com/jekyll/jekyll-import) to pull the posts from our old tech blog on Tumblr and create simple `meta` redirects for referrers linking to the rather non-semantic Tumblr URLs, helping prevent linkrot and moving the TC tech blog to a better canonicalized URL structure (and somewhere an SEO guy cheers). Once that was done I modified the Octopress config YAML and post frontmatter, finished up with various copy editing and post migration tasks, including adding the `CNAME` file and updating the DNS to point to the new home of the Trunk Club Tech Blog. And poof. The new blog came alive.

Let the blogging begin.