---
layout: post
title: Mirror a directory with SCP
date: 2013-05-20
---

Another short one today, courtesy of [Chris H](http://twitter.com/choult).

When working in a directory, if you need to copy everything from the current directory to
an identical path on another machine, you can do it as follows:

{% highlight bash %}
scp -r ./* remote.host:$PWD
{% endhighlight %}

This is also possible with rsync, but as the SCP syntax is much simpler it's a nice one liner to know.
