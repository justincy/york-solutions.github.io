---
layout: post
title:  "Affordable Email Forwarding For Small Projects"
date:   2015-09-23
---

The biggest frustration I've had recently as a developer is finding cheap email
forwarding. When working on a small project, such as [RootsSearch](https://rootssearch.io),
I want to have an email listed somewhere for anyone to contact me. I want that 
email to be forwarded to my personal inbox so that I don't have to check multiple
accounts with multiple hosts.

Domain registrars often offer free email forwarding, such as 
[Namecheap](https://www.namecheap.com/) or [Google](https://domains.google.com).
But I like to use [CloudFlare](https://www.cloudflare.com/) for my DNS and 
CloudFlare requires you to use their nameservers which means your registrar
can't control your MX records and therefore you need a separate solution for 
email forwarding.

When I first ran into this problem I figured the solution would be easy. A previous
employer had used [Dyn](http://dyn.com/email/dyn-email-forward/) and later
[DuoCircle](http://www.duocircle.com/services/email-forwarding) for email forwarding
so there was at least one company out there. But DuoCircle charges $59.95 a year
per domain. That quickly adds up for small projects, especially if you have multiple.

[33Mail](http://www.33mail.com/) looked promising, especially at only $1 a month
to use custom domains. But I quickly ran into a feature I couldn't tolerate nor
prevent: to every email I received, 33Mail added a banner that said 
"This email was sent to the alias... and 33Mail forwarded it to you." I didn't
mind so much seeing the banner in the emails I received, but I didn't want them to appear
in the quoted section of an email thread when I responded to customers. I could
easily remove them when replying (which I did) but it meant I was again on the
hunt for a good email forwarding solution.

[Pobox](https://www.pobox.com/pricing) was another promising candidate. It has
been around a while and was cheap at $20 a month. Though their volume limits 
seemed low and I didn't want to be paying for services that I wouldn't be using.

After _weeks_ of searching on and off, I finally found [Mailgun](https://mailgun.com)
by Rackspace. I was skeptical at first because it looked just like [Mandrill](http://mandrill.com/)
which doesn't support inbound email forwarding out of the box. But it turns out
that Mailgun _does_ support email forwarding. Their pricing is also perfect: 
10,000 emails every month for __free__. Sold.