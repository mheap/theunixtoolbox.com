---
title: Rename a git branch
layout: post
date: 2015-10-02
---

I can't count the number of times that I've created a new feature branch only to notice that I've managed to add a typo to the branch name. Usually, it looks something like this:

{% highlight bash %}
  $ git checkout -b feature/addign-login
{% endhighlight %}

Usually in this situation I'd create a new branch with the correct name from my current (incorrectly named) branch and delete the old one:
{% highlight bash %}
$ git checkout -b feature/adding-login

$ git branch -D feature/addign-login
{% endhighlight %}

It turns out that this happens enough for `git` to have an alias for it - `git branch -m`.

Instead of creating a new branch and deleting the old one, you can do the following:

{% highlight bash %}
  $ git branch -m feature/adding-login
{% endhighlight %}

This will rename the branch that you're on in place.

<hr />

If you want to see a more complete example, here I fix the typo running `git branch` before and after to show you the state of branches before and after

{% highlight bash %}
$ git branch
* feature/addign-login
  master

$ git branch -m feature/adding-login

$ git branch
* feature/adding-login
  master
{% endhighlight %}

You can of course use any name that you like
{% highlight bash %}
$ git branch
* feature/adding-login
  master

$ git branch -m i-like-turtles

$ git branch
* i-like-turtles
  master
{% endhighlight %}


