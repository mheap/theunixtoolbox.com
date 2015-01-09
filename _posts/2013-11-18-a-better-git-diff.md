---
title: A better git diff
layout: post
date: 2013-11-18
---

Whitespace is like `git diff`'s krypton, it makes changes that are actually tiny look much more complicated than they actually are. Thankfully, `git` comes with a few flags that you can use in conjunction with git diff to make life a bit easier.

The first option is `--ignore-space-at-eol`. This flag makes `git diff` ignore any changes to whitespace at the end of a line. Most developers have options to automatically trim trailing whitespace, but if you're working in a team that doesn't have it enabled you might find this option useful.

<hr />

{% highlight bash %}
  git diff --ignore-space-at-eol
{% endhighlight %}

The next flag is `-b`, which is an alias for `--ignore-space-change`. This is useful to use when someone goes through and converts tabs to spaces, or something similar. The whitespace hasn't been added or removed, it's just changed size. For most people reading a diff, that's not important (unless you're writing Python, that is).

{% highlight bash %}
  git diff -b
{% endhighlight %}

The final flag is `-w`, which is the same as `--ignore-all-space`. Imagine that we have a line with no whitespace at the beginning of a line, but we reindent and now it has spaces at the beginning. Using `git diff -b` would show this change as it's not a change of space, it's an addition. Using `git diff -w` will hide the change as the content of the line hasn't changed other than whitespace.

It's worth noting that even when using `-w`, the addition or removal of blank lines will still show in `git diff`. This is because the line didn't exist previously, but now it does.

The final thing to do is to add this new `git diff` to your `~/.gitconfig` file. I personally like `git diff -b` as it hides the majority of whitespace changes, but not quite as much as `-w` which could hide changes in indentation levels etc. To add `git wdiff` to your available git commands, add the following to your `~/.gitconfig` file:

{% highlight ini %}
[alias]
    wdiff = !git diff -b
{% endhighlight %}
