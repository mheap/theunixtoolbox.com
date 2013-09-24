---
title: jq - sed for JSON
layout: post
date: 2013-09-24
---

I can't remember the last time a day went by that I didn't end up working with JSON data. It's a lovely 
format to work with, but unfortunately due to how verbose it is it can be quite difficult to extract the
specific details that you're looking for.

`jq` is a standalone binary for working with JSON. Just download it, put it in your `$PATH` and get to
work. It has [all kinds of options available](http://stedolan.github.io/jq/manual/#Invokingjq), but I
tend to use it just for filtering data.

My most common use case is this:

{% highlight bash %}
  echo '{"foo":"bar","bees":true}' | jq .
{% endhighlight %}

`jq` reads the JSON from stdin and runs the `.` filter (aka "match everything") against it. This results
in a nicely formatted JSON representation for me to read.

Sometimes though, the output I'm using is a bit big to search through by hand. This is where `jq` [filters](http://stedolan.github.io/jq/manual/#Basicfilters) come in useful. Using the same JSON as last time, I want to see the output of "foo".

{% highlight bash %}
  echo '{"foo":"bar","bees":true}' | jq .foo
{% endhighlight %}

Things are starting to get a bit more complicated now, but still not too complicated that I couldn't look at it by hand. Let's take a look at what happens when we get arrays of data involved. We want to go through each array item and pull out "foo". So, from our root (`.`), look at each array item (`[]`), and pull out "foo" (`.foo`). This makes our filter `.[].foo`.

{% highlight bash %}
  echo '[{"foo":"bar","bees":true},{"foo":"baz","bees":true},{"foo":"foo","bees":true},{"foo":"bee","bees":true}]' | jq ".[].foo"
{% endhighlight %}

That's pretty much the extent of my experience with `jq`. There's a whole host of advanced features that are explained well in [the manual](http://stedolan.github.io/jq/manual/).

There's plenty of ways to [install jq](http://stedolan.github.io/jq/download/) - for most people it's just a case of downloading the binary. If you're on a Debian based OS it's available in `apt`, or if you're on OSX it's on `brew`.
