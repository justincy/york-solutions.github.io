---
layout: post
title:  "Managing Language Packs in a JavaScript Library"
date:   2015-04-02
---

I'm in the process of adding support for multiple languages to [FSCheck][fs-check],
but I'm stuck on how to manage the different languages.

I'm trying to support using FSCheck in both node and the browser, now that the
[FamilySearch JavaScript SDK][fs-sdk] supports both. That means I need a build
process that packages up the main library and packages the languages separately
so that, in the browser, you can selectively download and include the languages.
In node, however, disk space is not really an issue so I want all languages
to be automatically included.

Another requirement is for all the built language packs to be readily accessible
to the demo page hosted on [GitHub Pages][gh-pages]. The restriction here is that
the files must be served statically and therefore included in the gh-pages
branch, or served from elsewhere.

I began by having the English language in a [separate repository][fs-check-en].
There were lots of files and it had its own build process, so a separate repo
just made sense. But that made it hard to test with the main library, and
especially to include in the demo hosted on GitHub pages. I also didn't like the
idea of users going to n+1 different repos to download all the main library and
the different languages they wanted to use.

While working on some of the date features of FSCheck, I noticed that 
[Moment.js][moment.js] includes all of their languages in their main repo. This
is appealing because it makes maintenance, testing, and distribution easier. 
However, Moment's language files are much smaller and they don't require a build
process.[^1] 

Presently, the descriptions for each check is stored in separate markdown
files. As the number of checks grows,[^2] so will the number of files in each
language. The concern here is that git does not manage large numbers of files
well. In the future, if we have 30 languages and 100 checks then that will be
3,000 language description files plus their history. But that isn't anywhere close
to [larger repos which work well][large-repos]. 

I began working in this direction but had some lingering questions:

1. How will the build work? If I run the build on the main lib, should it also
   build the languages? Should I make it easy to build one language at a time?

2. Will the tests reside in the language directories on in the main test
   directory? Should I make it easy to test one language at a time?

I ended up adding a package.json file to the English directory to describe how
the package will be managed (entry point for node and the build script). Then
I realized I was close to a git submodule; it had all the pieces except for a
separate .git directory.

GitHub supports [using submodules with pages][submods-pages]; it will automatically
install submodules when deploying the pages. That solves the requirement of
having the language packs available for the demo, but it complicates development
and usage in node. The developer now needs to remember to properly download
and manage the submodules (which is not always easy). We can simplify the matter
with npm install hooks and other scripts, but that leads to high levels of black
magic and further complicates the setup. For useage in node, we would be replacing
the normal npm dependencies with submodules which just feels wrong. Why go to all
this work and maintain separate repos if they're already separate repos to begin
with? There was too much added complexity and anti-patterns to justify this method.

I've settled on including the language files directly in the main repo. The
language packs are heavily dependent on the main library, should be versioned in
step, and need to be readily accessible. The main fear of having a bloated repo
is not valid and I can't see any other downsides to it.

[fs-check]: https://github.com/genealogysystems/fs-check
[fs-check-en]: https://github.com/genealogysystems/fs-check-en
[fs-sdk]: https://github.com/rootsdev/familysearch-javascript-sdk
[moment.js]: http://momentjs.com/
[large-repos]: http://stackoverflow.com/a/984973/879121
[gh-pages]: https://pages.github.com/
[submods-pages]: https://help.github.com/articles/using-submodules-with-pages/

[^1]: Whether I should be using a build process for the language packs is worth considering in a separate post.
[^2]: There are already many on the [backlog](https://github.com/genealogysystems/fs-check/issues).