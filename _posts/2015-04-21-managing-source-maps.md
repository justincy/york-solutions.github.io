---
layout: post
title:  "Managing Source Maps"
date:   2015-04-21
---

[Find-A-Record][far] relies on a host of open-source JavaScript libraries. 
It is tedious to update them, especially when using minified versions with source maps.

Source maps link the minified code to the un-minified source code.
The minified code includes a special comment which points to the source map.

{% highlight js %}
//# sourceMappingURL=/path/to/file.js.map
{% endhighlight %}

The browser follows the `sourceMappingURL` to download the source map which means
the map needs to be available. The best idea is to store the source map in the
same directory as the minified file which is what most (if not all) JS libs expect.

3. The source map points to the original source (which also must be available
   somewhere) and contains a mapping from the minified code to the original.

This puts a few undesirable restrictions on us.

### 1. Manage two additional files per library

That means we need three files to be publicly available for each minified JS lib
that we want to debug: the minified version, the source map, and the original source.

We manually download all external JS libs into the `assets/vendor/js` folder and
commit them directly into the website's repo.[^1] This was okay when each JS lib
required only one file to be downloaded; somehow 3 makes a big difference.

### 2. Can't change the file names

We prefer to include the version number of the library in the filename,
uch as `jquery-2.0.0.min.js` so that we can quickly tell which version we're using. 
And it would allow us to use multiple versions of a library if we ever needed to.[^2] 
But mostly it makes management of the libraries easier.

Changing file names can break source maps by breaking the pointers
stored in the minified file to the source map and in the source map to the original
source.

### Others feel the pain

These issues are so painful that jQuery [stopped enabling source maps][jquery-maps] by default
in the 1.11 and 2.1 releases. jQuery suggests that developers edit the minified
file by hand to add the source map comment if they need it. That works well for
something like jQuery which you don't normally update often nor need to debug
on your website but that's not my situation. Editing files by hand makes point #1
even more painful.

I would prefer to see one of jQuery's suggestions implemented by browsers:[^3]

> Make it possible for users to specify a map file manually via a URL in dev tools. 
> This would allow us to ship a file without a sourcemap comment but the user could add the association themselves if they use maps.

That would solve a lot of my pain with source maps because I don't actually need
them that often. I only need them when diagnosing bugs that surface in our error
logs. I open the URL where the error occurred on our site, find the conditions
that reproduce the error and isolate it the most simple and quickly reproducible
situation, then use the debugger to diagnose it. It's not until I open the debugger
that I actually need the source map. Perhaps then I _should_ follow jQuery's lead
and not have them enabled by default but just include them as necessary.

[far]: https://www.findarecord.com
[jquery-maps]: http://bugs.jquery.com/ticket/14415

[^1]: We haven't used a package manager like bower yet because it wasn't until recently that we started feeling any pain.
[^2]: Heaven forbid. But there can be legitimate reasons for it.
[^3]: I only really care about Chrome.