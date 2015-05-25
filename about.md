---
layout: default
title: About
permalink: /about/
---

{% highlight javascript %}
{ 
  "name": "Richard Clark",
  "occupation": [ "Sysadmin", "Developer" ],
  "location": "London",
  "contact": [
    "email": "<firstname-at-fohnet.co.uk>"
  ]
}
{% endhighlight %}

## Links

* [PGP key][gpg-key]
* [github][github-rdark]
* [keybase][keybase-rdark]
* [pinboard][pinboard-rdark]
* [blog feed][blog-feed]

[pinboard-rdark]: https://pinboard.in/u:rdark
[github-rdark]: https://github.com/{{ site.github_username }}
[keybase-rdark]: https://keybase.io/rdark
[gpg-key]: {{ site.baseurl }}/pgp.txt
[mailto-rdark]: mailto://{{ site.contact_email }}
[blog-feed]: {{ "/feed.xml" | prepend: site.baseurl }}
