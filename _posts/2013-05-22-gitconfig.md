---
layout: post
title: .gitconfig
date: 2013-05-22
---

When working with `git`, there's quite a few different settings that you can set to change
your experience. A lot of people know about the defaults of "user.name" and "user.email" for 
identification, but there's loads more that you can set up.

Below, is a (mostly complete) copy of my `~/.gitconfig` file. As well as identifying myself, I've 
added a few useful aliases, added colouring and configured which utilities to use for common actions.

My favourite option is probably "help.autocorrect". Without it, git suggests a command close to 
the one you typed, but makes you type it yourself. With this enabled, it just runs it:

{% highlight bash %}
$ git stats
WARNING: You called a Git command named 'stats', which does not exist.
Continuing under the assumption that you meant 'status'
in 0.1 seconds automatically...
{% endhighlight %}

As well as that, I force any repositories that I own to be checked out as read/write, rather 
than read only. It's something I don't do much, but I don't even have to worry about it now.

{% highlight ini %}
# Any GitHub repo with my username should be checked out r/w by default
# http://rentzsch.tumblr.com/post/564806957/public-but-hackable-git-submodules
[url "git@github.com:mheap/"]
insteadOf = "git://github.com/mheap/"
{% endhighlight %}

When looking at the alias section, you might notice that some of the commands start with a "!". 
This means "run the following as a shell command". As it can be anything that can be executed, 
you can even provide a path to a script to run. In my `.gitconfig`, I run a script from `~/.dotfiles/bin`. 
I say use the `$ZSH` variable from the environment as the base path for the script. Anything you can do 
on the CLI, you can do in your `.gitconfig`.

{% highlight ini %}
[alias]
wtf = !$ZSH/bin/git-wtf
{% endhighlight %}

You can find all of the git scripts in my [dotfiles repository](https://github.com/mheap/dotfiles/tree/master/bin). They're the ones that start with `git-`.

### Full .gitconfig

{% highlight ini %}

# This is me
[user]
name = Michael Heap
email = m@michaelheap.com

# Set up some useful aliases
[alias]
co = checkout
ci = commit
sl = !git shortlog -sn
lg = !git log --graph --pretty=oneline --abbrev-commit --decorate
wtf = !$ZSH/bin/git-wtf

# Add colour to ALL THE THINGS
[color]
diff = auto
status = auto
branch = auto
ui = true

[core]
# Set up a global excludes file
excludesfile = ~/.gitignore
# Which apps do I want to use with git?
editor = vim
pager = less -r

# Don't warn about whitespace conflicts when applying a patch
[apply]
whitespace = nowarn

# Autocorrect anything I typo
[help]
autocorrect = 1

# Any GitHub repo with my username should be checked out r/w by default
# http://rentzsch.tumblr.com/post/564806957/public-but-hackable-git-submodules
[url "git@github.com:mheap/"]
insteadOf = "git://github.com/mheap/"

{% endhighlight %}

### .gitignore

For completeness, here's my personal `~/.gitignore` file too

{% highlight text %}
.DS_Store
.svn
*~
*.swp
*.rbc
{% endhighlight %}

