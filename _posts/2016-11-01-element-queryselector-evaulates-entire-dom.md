---
layout: post
title: "Element.querySelector() Evaluates the Entire DOM"
date: 2016-11-01
---

I used to think that [`Element.querySelector()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelector)
and [`Element.querySelectorAll()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelectorAll)
worked like [`jQuery.find()`](https://api.jquery.com/find/) in that they would
evaluate a CSS selector against an element's descendants. But today I learned
that they are not the same.

Instead of evaluating selectors against an element's descendants, `querySelector()`
and `querySelectorAll()` evaluate selectors against the entire DOM then filter
the results to those that are descendants of the base element.

Consider the following example, inspired by MDN.

```html
<div>
  <p>Some <span>paragraph</span> text.</p>
</div>
```

```js
console.log( $('p').find('div span').length ); // prints 0

console.log( document.querySelector('p').querySelectorAll('div span').length ); // prints 1
```