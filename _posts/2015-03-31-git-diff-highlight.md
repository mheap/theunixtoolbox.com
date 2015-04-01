---
title: git and diff-highlight
layout: post
date: 2015-03-31
---

diff-highlight is a contrib script that ships with git. It's a better way to visualise a diff when the changes are small words, not entire lines/paragraphs. It's hard to explain, so here's an example (`diff-highlight` is the script):

<hr />

#### Standard git diff
This is `git diff` as we know it.

{% highlight bash %}
  $ git diff 
{% endhighlight %}

![git diff](/post-images/git-diff-highlight/git-diff.png)

#### Add diff-highlight
Let's add `diff-highlight`. This makes it easier to see the changes, but we've lost our colours. This is because `git` disables colours when output is piped to another command.

{% highlight bash %}
  $ git diff | diff-highlight
{% endhighlight %}

![git diff | diff-highlight](/post-images/git-diff-highlight/git-diff-with-highlight.png)

#### diff-highlight and colours

To get the colours back, we add the `--color` flag to `git diff` to force colour output.

{% highlight bash %}
  $ git diff --color | diff-highlight
{% endhighlight %}

![git diff](/post-images/git-diff-highlight/git-diff-with-highlight-color.png)

#### Large diffs

If you're looking at quite a large diff, you might want to pipe it into `less`. By default, `less` doesn't support colours. Fortunately this is easy to enable by adding the `-r` flag.

{% highlight bash %}
  $ git diff --color | diff-highlight | less -r
{% endhighlight %}

## Installation

To install diff-highlight, you'll need to get a copy of [the script](https://github.com/git/git/blob/master/contrib/diff-highlight/diff-highlight) and put it somewhere in your `$PATH`. You'll also need to make the script executable.

If you have `~/bin` in your path, here's a one liner to install:

{% highlight bash %}
$ curl https://raw.githubusercontent.com/git/git/master/contrib/diff-highlight/diff-highlight > ~/bin/diff-highlight && chmod +x ~/bin/diff-highlight
{% endhighlight %}

## Configuration
If you like this behaviour and want to use it frequently, it may be worth configuring your `git` installation for easy access. All of these changes are made in `~/.gitconfig`.

The easiest way to do it is to enable it for everything by changing your pager setting so that all output is piped through `diff-highlight` before being passed to your pager. This is what it looks like for me.

{% highlight ini %}
[core]
  pager = diff-highlight | less -r
{% endhighlight %}


If you'd rather just enable it for certain actions, it's easy to add as an alias. I've added the following to my `.gitconfig` and use it with `git worddiff`.

{% highlight ini %}
[alias]
    worddiff = !git diff --color | diff-highlight | less -r
{% endhighlight %}
