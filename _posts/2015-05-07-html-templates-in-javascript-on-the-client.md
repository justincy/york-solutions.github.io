---
layout: post
title:  "Using HTML Templates with JavaScript on the Client"
date:   2015-05-07
---
Generating HTML in JavaScript can be painful. It quickly gets out of control
when building any advanced UI. 

Let's say we want to display a contact list using a list of contacts with this schema:

{% highlight js %}
{
  name: 'Joe',
  img: '/pics/Joe.png',
  phone: '987-654-3210'
}
{% endhighlight %}

We want the resulting HTML to look like this:

{% highlight html %}
<div class="contacts">
    <div class="contact">
        <img class="thumb" src="/pics/Joe.png" />
        <div class="name">Joe</div>
        <div class="phone">987-654-3210</div>
    </div>
    <!-- more contacts... -->
</div>
{% endhighlight %}

### Vanilla JavaScript

Traditionally, with vanilla JS, we generated elements directly. [jsFiddle][fiddle1]

{% highlight js %}
var list = document.createElement('div');
list.className = 'contacts';
for(var i = 0; i < contacts.length; i++){
    var contact = document.createElement('div');
    contact.className = 'contact';
    
    var img = document.createElement('img');
    img.src = 'http://lorempixel.com/50/50/people/' + contacts[i].name;
    img.className = 'thumb';
    contact.appendChild(img);
    
    var name = document.createElement('div');
    name.className = 'name';
    name.innerHTML = contacts[i].name;
    contact.appendChild(name);
    
    var phone = document.createElement('div');
    phone.className = 'phone';
    phone.innerHTML = contacts[i].phone;
    contact.appendChild(phone);
    
    list.appendChild(contact);
}
document.body.appendChild(list);
{% endhighlight %}

Using vanilla JS is a lot of work; 23 lines of code to generate our simple contact
list. It's also error prone because of cross-browser compatibility issues.

### jQuery

[jQuery][jquery], released in 2006, solved our browser compatibility issues
and made DOM manipulation easier. [jsFiddle][fiddle2]

{% highlight js %}
var list = $('<div>').addClass('contacts');
for(var i = 0; i < contacts.length; i++){
    var contact = $('<div>').addClass('contact');
    
    var img = $('<img>')
        .attr('src', 'http://lorempixel.com/50/50/people/' + contacts[i].name)
        .addClass('thumb')
        .appendTo(contact);
    
    var name = $('<div>')
        .addClass('name')
        .text(contacts[i].name)
        .appendTo(contact);
    
    var phone = $('<div>')
        .addClass('phone')
        .text(contacts[i].phone)
        .appendTo(contact);
    
    list.append(contact);
}
$(document.body).append(list);
{% endhighlight %}

This is about the same number of lines of code as vanilla JS, though there
are many less characters which I say leads to enhanced readability. We also
don't have to worry about browser compatibility.

### Handlebars

We can do better. Instead of building the HTML in JavaScript, let's separate the
HTML from the JavaScript in the same way we separate CSS from HTML. We'll use
[Handlebars][handlebars] to create HTML templates which are later compiled and 
populated.

First, our HTML template is embedded in a script tag in our HTML page. 

{% highlight html+handlebars %}
{% raw %}
<script id="contacts-template" type="text/x-handlebars-template">
    <div class="contacts">
        {{#each contacts}}
        <div class="contact">
            <img class="thumb" src="http://lorempixel.com/50/50/people/{{name}}" />
            <div class="name">{{name}}</div>
            <div class="phone">{{phone}}</div>
        </div>
        {{/each}}
    </div>
</script>
{% endraw %}
{% endhighlight %}

Then in JavaScript we retrieve, compile, and evaluate our template. [jsFiddle][fiddle3]

{% highlight js %}
var source = document.getElementById('contacts-template').innerHTML;
var template = Handlebars.compile(source);
document.body.innerHTML = template(data);
{% endhighlight %}

That is much better. Our HTML is in a template which makes refactoring the HTML
much easier (imagine trying to refactor the HTML construction in the first two examples).
We also get syntax highlighting for the HTML when in our IDE. And now our JavaScript
has been reduced from 23 lines to 3 lines for this simple example (the savings can
be much larger for more complex projects).

Checkout the jsFiddles for complete and working examples of the three usecases
we presented here: [vanilla JS][fiddle1], [jQuery][fiddle2], and [Handlebars][fiddle3].

[jquery]: http://jquery.com/
[handlebars]: http://handlebarsjs.com/
[multiline]: http://stackoverflow.com/a/6247331/879121
[fiddle1]: http://jsfiddle.net/g00xzpsf/
[fiddle2]: http://jsfiddle.net/et1zt7uj/
[fiddle3]: http://jsfiddle.net/e2Lonvh7/1/