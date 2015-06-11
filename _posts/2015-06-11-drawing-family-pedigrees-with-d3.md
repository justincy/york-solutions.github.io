---
layout: post
title:  "Drawing Family Pedigrees With D3"
date:   2015-06-11
---

I recently set out to learn how to draw family pedigrees with [D3][d3]. Their
website is full of examples including an actual [pedigree chart][d3-pedigree-example].
That example was much to simple for my requirements. I wanted it to be styled
like most pedigrees in software with a box around the person. I also wanted it
to expand, collapse, pan, and zoom. As I perused other D3 examples, I had difficulty 
mixing and matching techniques and separating the basics from the fancy stylistic 
additions. I decided to document and share what I learned in a incremental set 
of examples so that you wouldn't have to become an expert in D3 just to draw a pedigree.

The [D3 pedigree examples][my-pedigree-examples] are hosted on GitHub. At the time
of writing, they include 5 examples:

* [Basic](http://justincy.github.io/d3-pedigree-examples/basic.html): A basic static pedigree.
* [Expand and Collapse](http://justincy.github.io/d3-pedigree-examples/expandable.html): Click on persons to expand and collapse the tree.
* [Smooth Transitions](http://justincy.github.io/d3-pedigree-examples/transitions.html): Changes are animated when the tree is expanded or collapsed.
* [Ancestors and Descendants](http://justincy.github.io/d3-pedigree-examples/descendants.html): Show both ancestors and descendants.
* [Ancestors and Descendants - OOP](http://justincy.github.io/d3-pedigree-examples/descendants-oop.html): A more sane example of ancestors and descendants using classes.

Visit [the repo][my-pedigree-examples] to learn more about the implementation details.
Here, I want to share my thoughts about whether D3 is a good tool for drawing pedigrees.

1. [D3][d3] is a beautiful and powerful library. It's too powerful for most people
to tame. Without being able to copy and paste from the enormous library of examples,
most users would not be able to get anything done[^1]. The moment you need to modify
anything, you have to become an expert in both SVG and D3. This will limit the
usefulness of the examples I created.

2. SVG is great for shapes with a little bit of text. But pedigrees are lots of
text with a few shapes. SVG does not support text wrapping nor inline styles (the
ability to style one word, character, or phrase different than the other text
around it). Some browsers allow you to embed HTML in a `foreignObject` so that
you can use HTML and CSS to style text, which has all the tools we need, but 
support is partial for those browsers that allow it while no version of IE 
supports it. That renders the `foreignObject` useless for the majority of production
projects.

3. The complexity increases by orders of magnitude if you want to make the D3
visual dynamic and interactive (in our case expand and collapse).

4. D3 does lend itself well to an OOP approach. This is why the examples are 
generally one large wall of code.

[More to come about what a better alternative is...]

[d3]: http://d3js.org/
[d3-pedigree-example]: http://bl.ocks.org/mbostock/2966094
[my-pedigree-examples]: https://github.com/justincy/d3-pedigree-examples

[^1]: I'm not sure I could've done anything either.