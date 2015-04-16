---
layout: post
title:  "JavaScript Error Logging With Track:JS"
date:   2015-04-16
---

I first discovered [{TrackJS}][trackjs] about a year ago while browsing the list 
of [integrations][segment-integrations] available for [Segment.io][segment]. 
I quickly realized that TrackJS solved a problem which I didn't know I had.

At the time I was working full-time on [Find-A-Record][findarecord], a JavaScript
intensive web application. I realized that I had no insight into whether our users
were running into any errors. TrackJS solves this by listening to global error
events, wrapping the console, and listening for Ajax errors. It sends all that
data to its server with additional metadata and stack traces (when available)
for quick browsing and reviewing on their website.

Setup is simple: create an account then add a few lines of JavaScript to your
page, just like other JavaScript tracking libraries such as [Google Analytics][ga].
TrackJS handles the rest automatically for you. Review the [documentation][docs] for
more details on how it works and what customization options are available.

{% highlight js %}
<!-- BEGIN TRACKJS -->
<script type="text/javascript">window._trackJs = { token: 'YOUR_TOKEN' };</script>
<script type="text/javascript" src="//d2zah9y47r7bi2.cloudfront.net/releases/current/tracker.js" crossorigin="anonymous"></script>
<!-- END TRACKJS -->
{% endhighlight %}

TrackJS immediately began providing us with useful insight into errors that our
users were seeing, such as exceptions thrown when trying to use local storage
or errors in a 3rd party API that Find-A-Record uses.

TrackJS offers a 30-day free trial and plans starting at $29.99 a month. They
also have a free tier which only keeps a 24-hour history of errors.

[trackjs]: https://trackjs.com/
[segment]: http://segment.io
[segment-integrations]: https://segment.com/integrations
[findarecord]: htps://findarecord.com
[ga]: http://www.google.com/analytics
[docs]: https://docs.trackjs.com/