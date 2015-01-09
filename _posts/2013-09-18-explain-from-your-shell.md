---
layout: post
title: Explain, from your shell
date: 2013-09-18
---

A few weeks ago, almost everyone on Twitter was sharing [Explain Shell](http://explainshell.com/) - with good reason. It's an awesome site that you can copy and paste a command into and it'll explain each component of it to you.

There are a few examples on the homepage, I particularly like  [tar xzvf archive.tar.gz](http://explainshell.com/explain/tar?args=xzvf+archive.tar.gz) and [ssh  -i keyfile -f -N -L 1234:www.google.com:80 host](http://explainshell.com/explain/ssh?args=-i+keyfile+-f+-N+-L+1234%3Awww.google.com%3A80+host).

<hr />

Now, whilst you can copy and paste a command you're working on into the site, wouldn't it be awesome if you could trigger it from your command line? Thanks to Schneems, [you can!](http://schneems.com/post/61514247453/explain-shell-from-your-shell). I prefer the [shell version](https://github.com/schneems/explain_shell#without-rubygems), so add the following to your `.bashrc` (or `.zshrc` etc) and then next time you want to see what a command does, just add `explain` to the beginning of it.

{% highlight bash %}
function explain {
  # base url with first command already injected
  # $ explain tar
  #   => http://explainshel.com/explain/tar?args=
  url="http://explainshell.com/explain/$1?args="

  # removes $1 (tar) from arguments ($@)
  shift;

  # iterates over remaining args and adds builds the rest of the url
  for i in "$@"; do
    url=$url"$i"+"
  done

  # opens url in browser
  open $url
}
{% endhighlight %}
