---
layout: post
title:  "Using toJSON to Customize an Object's Serialization"
date:   2015-03-27
---

Circular references are common in data structures. For example, parent-child
relationships where a parent has pointers to all of its children and children
have pointers to their parents, or [doubly-linked lists][double-list] where
each entry has a pointer to both the next and the previous entry. However,
JSON does not support circular references. If you try to convert an object with
circular references into JSON you will get a type error.

{% highlight js %}
var a = {};
var b = {friend: a};
a.friend = b;
JSON.stringify(a);

> TypeError: Converting circular structure to JSON
{% endhighlight %}

I ran into this error while trying to serialize objects from the
[FamilySearch JavaScript SDK][fs-sdk]. Being one of the major contributors to
that project, I knew the circular references were caused by the SDK internals
and not the actual data that I wanted, so I needed a way to filter out the 
internal objects while maintaining the useful data.

It turns out that [JSON.stringify()][stringify] accepts an option replacer 
function as its second argument.

> A function that alters the behavior of the stringification process, or an 
> array of String and Number objects that serve as a whitelist for selecting 
> the properties of the value object to be included in the JSON string. If this 
> value is null or not provided, all properties of the object are included in
> the resulting JSON string.

Knowing that the references to the SDK internals were always prefixed with a `$`,
I could easily filter them out like this:

{% highlight js %}
JSON.stringify(object, function(key, value){
  if(key.indexOf('$') === 0){
    return;
  }
  return value;
});
{% endhighlight %}

It was nice to know I could do that, but it was too hacky; it only worked because 
I knew some of the internals of the SDK. I wanted a solution that worked without
inside knowledge. I wanted something like [toString()][tostring] for JSON that
could be configured by the SDK itself so that all users had a nice way to serialize
objects returned by the SDK.

I was happy to learn that such a property exists and that it's conventiently
named [toJSON][tojson].

> If an object being stringified has a property named toJSON whose value is a 
> function, then the toJSON() method customizes JSON stringification behavior: 
> instead of the object being serialized, the value returned by the toJSON()
> method when called will be serialized.

Now I can add the following code to the SDK and make it easy for all users of 
the SDK to seralize.

{% highlight js %}
FS.BaseClass.prototype.toJSON = function(){
  var obj = {};
  for(var a in this){
    if(a.indexOf('$') !== 0){
      obj[a] = this[a];
    }
  }
  return obj;
};
{% endhighlight %}

[fs-sdk]: https://github.com/rootsdev/familysearch-javascript-sdk
[double-list]: http://en.wikipedia.org/wiki/Doubly_linked_list
[stringify]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify
[tostring]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString
[tojson]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#toJSON_behavior