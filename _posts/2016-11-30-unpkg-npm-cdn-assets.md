---
layout: post
title: "unpkg - CDN for npm packages"
date: 2016-11-30
---

Now we can stop the deadly sin of committing our assets to git for 
[Bower](https://bower.io/), [CDNJS](https://cdnjs.com/), and 
[jsDelivr](http://www.jsdelivr.com/). Instead let's use [unpkg](https://unpkg.com/#/), 
a CDN for npm.

We typically think of npm as a package manager for node modules written in
plain JS that will run without any compilation, but npm is actually built to
support the publishing of compiled and packaged assets. This is useful for
compiling CoffeeScript or transpiling ES2015. In fact, npm allows us to publish
anything. So lets publish our client-side assets there too.

[unpkg](https://unpkg.com/#/) is a CDN that runs in front of npm. Any package
published on npm is available through unpkg. Assets are accessed through URLs
using this simple format: `https://unpkg.com/package@version/file`. For example,
`https://unpkg.com/lodash@4.17.2/lodash.min.js`.

1. Generate a UMD package using your favorite build tool.
2. Add your packaged assets to `.gitignore` so it's not commited to git.
3. Add your packaged assets to the `files` array in `package.json` so they're
part of the package published to npm.

It's simple and beautiful. [unpkg](https://unpkg.com/#/) is now my preferred CDN.