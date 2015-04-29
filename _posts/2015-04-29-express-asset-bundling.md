---
layout: post
title:  "Asset Bundling in Express"
date:   2015-04-29
---

[Find-A-Record][far] is built on [Express][express], a web framework for Node.js.
Remarkably, [Find-A-Record][far] has no tests, no build system, and no asset bundling 
system. That's okay for the static pages such as the home page, but the main web 
app relies on 15 JavaScript files and 3 CSS files in addition to the 3 JavaScript 
files and 3 CSS files included on every page in the site. It's unwieldy.

Including so many static assets has undesirable consequences. First, it increases
page load times because each asset needs to be downloaded separately.[^1] Each
request incurs the overhead of HTTP, TCP, and the limitations of the speed
of light as it travels over the wire.

A cascading consequence of having so many assets is that we don't want to add more.
Therefore, the majority of the custom JavaScript is kept in one 1,500 line file.
We don't want to split it up because it just adds more asset files for downloading.

We need a way to bundle our assets. If the assets are bundled then we reduce the
overhead of 24 total requests to 4: global CSS file, global JS file, page-specific
CSS file, and page-specific JS file.

The main reason why we've been avoiding a build or bundle system is that it's a
big commitment; you have to buy into a specific workflow and hope it scales well
into the future with your code base. Does it support minifying of JS and CSS?
Does it have a development mode where you can include unbundled and unprocessed
versions of your assets (very helpful for debugging)? Does it require bundled and
processed files to be committed to your repository? Does bundling occur when the
app is started? Does the app need to be restarted when source files are changed?

Since we're using Express, middleware was our first thought but all we could find
were [BundleUp][bup] and [Pound][pound] (which is built on BundleUp). We have
successfully integrated bundle-up but we know it's not a long-term solution 
since it's not being maintained and its dependencies are way out of date.

What we like about BundleUp is that bundles are configured inside of the app itself.
This makes it easier to figure which assets are included in which views and makes
the development mode possible where assets are included directly instead of bundled.
If you use an external bundling system and you want a development mode then bundles
need to be configured both internally and externally which can be a mess.

One downside of BundleUp is that it runs when the app starts so it slows startup time.
The documentation also leaves a lot to be desired. We noticed it was touching our
source files and causing [Nodemon][nodemon] to consistently restart the app.
It turns out that it's written to "compile" all assets, even if they're not being
processed (such as SASS or CoffeeScript) or minified. The default compile behavior
for vanilla JS and CSS is to copy but our paths were configured such that it was
writing over the originals with an exact copy.

We don't like [Bower][bower] because of it's reliance on manifest files and how you can
only exclude files in the manifest as opposed to marking only a few for inclusion.
Besides, Bower is designed to handle and bundle external dependencies. All of
the assets we want to bundle are already installed locally and committed to the repo.
We just want to process local files and bundle them all together.

[Browserify][browserify]'s strong point is for bundling CommonJS modules in a
way that enables them to be used in the browser. This allows you to right
JavaScript code in Node (leveraging Node's handy require syntax for modules)
and then package it up for the front end. We use Browserify for other projects
but that's not our use case here.

[Duo][duo] is a new kid on the block and fits our development style a little better, 
but again it's likely much more than we need.

In the future we hope to create our own middleware for Express. All it needs to
do is allow for bundles to be configured and then process and minify them. It
can't be too hard, and we can't believe [BundleUp][bup] is the only thing out there
like it.

[far]: https://www.findarecord.com
[express]: http://expressjs.com/
[bup]: https://github.com/FGRibreau/bundle-up
[pound]: https://github.com/FGRibreau/pound
[duo]: http://duojs.org/
[bower]: http://bower.io/
[browserify]: http://browserify.org/
[nodemon]: https://github.com/remy/nodemon

[^1]: I can't wait until [HTTP2](https://http2.github.io/) is more ubiquitous.