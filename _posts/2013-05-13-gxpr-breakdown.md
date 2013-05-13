---
layout: post
title: Weekly Breakdown - gxpr
date: 2013-05-13
---

Last week, we discovered [gxpr](http://theunixtoolbox.com/gxpr/). Using a script to make life easier is good, but understanding why it works is even better. Let's do a line by line breakdown of `gxpr` and try to understand how it works.

First, we add a [shebang](http://en.wikipedia.org/wiki/Shebang_(Unix\)) to give the shell a clue which interpreter to run it through.
{% highlight bash %}
#!/bin/sh
{% endhighlight %}

The we set up a couple of variables. The first is the `curl` command that we want to use. The second and third are URL's that we're going to pass in as parameters at a later date
{% highlight bash %}
CURL='curl -s --header User-Agent:gxpr/1.0'
GOOGLE="http://www.google.com/ig/calculator"
WOLFRAM="http://www.wolframalpha.com/input/"
{% endhighlight %}

We want to grab all of the arguments passed in and use them as one string as the parameter to Google Calculator. This line's pretty complicated, so let's break it down.
{% highlight bash %}
EXPR=$(echo "$@" | perl -MURI::Escape -ne 'chomp;print uri_escape($_)')
{% endhighlight %}

We start by executing the code in a sub shell so that we don't change anything in our current shell session. We know this because the command is surrounded by `$()`. The data returned from this expression is assigned to `EXPR` for use later. 
{% highlight bash %}
EXPR=$(...)
{% endhighlight %}

In a script `$@` means all arguments that were passed in, so we echo them out (surrounded by quotes) and use that as the input for the `perl` command, via the pipe `|` character.
{% highlight bash %}
... echo "$@" | perl ...
{% endhighlight %}

We call `perl`, passing the `-M` option to load the "URI::Escape" module and the `-n` and `-e` flags.

The `-e` flag allows you to specify the code to run as an argument, rather than passing in a filename.

The `-n` flag creates an implicit loop, meaning that the code you provide will run for as long as there is input, ensuring that all input is captured.

The script that we pass in uses `chomp` to trim whitespace off the end of the request, then escapes the input so that it safe to put into a URL. The `$_` is a special variable that is implicitly assigned the value of what came via stdin in this case.

{% highlight bash %}
... perl -MURI::Escape -ne 'chomp;print uri_escape($_)') ...
{% endhighlight %}

The next thing to do is to make the `curl` request and try and get our output. Again, there's quite a lot going on so let's break it down line by line.
{% highlight bash %}
res=$(
  $CURL "$GOOGLE?q=$EXPR" |
  perl -ne '/rhs: "?([^\[\],":]+)/ and print $1' |
  perl -pe 's/[^\x00-\x7F]//g'
)
{% endhighlight %}

Execute in in a sub shell so that we don't affect our current session
{% highlight bash %}
res=$(...)
{% endhighlight %}

Make the `curl` request using the variables we defined earlier.
{% highlight bash %}
... $CURL "$GOOGLE?q=$EXPR" ...
{% endhighlight %}

This expands to:
{% highlight bash %}
... curl -s --header User-Agent:gxpr/1.0 "http://www.google.com/ig/calculator?q=$EXPR" ...
{% endhighlight %}

Google calculator returns JSON output ([example](http://www.google.com/ig/calculator?q=1%2B1)), so if we search for "1+1" we get the following output:
{% highlight javascript %}
{lhs: "1 + 1",rhs: "2",error: "",icc: false}
{% endhighlight %}

We want to extract the "rhs" section, which we can do with the following regular expression. The regex reads as following: "Find 'rhs: ', then an option quotation mark. Next, find everything until we hit one of the following characters: `[],":`. If we have any output from this, print it out."
{% highlight bash %}
...perl -ne '/rhs: "?([^\[\],":]+)/ and print $1' ...
{% endhighlight %}

Once we have that return value, strip any non-ascii characters from it
{% highlight bash %}
...  perl -pe 's/[^\x00-\x7F]//g' ...
{% endhighlight %}

Now that we've grabbed whatever Google gave back to us, it's time to make sure that we actually got a return value.
{% highlight bash %}
test -z "$res" && {
    echo "google doesn't know" "$@" 1>&2
    echo "⌘ click: \033[4m$WOLFRAM?i=$EXPR\033[0m"
    exit 1
}
{% endhighlight %}

`test` is a utility that return true or false. There's loads of options, which you can see by running `man test`. There is also a more commonly used alternative syntax, represented by `[[ -z "$res" ]]`. The `-z` flag means "evaluate to true if the string provided is empty".
{% highlight bash %}
... test -z "$res" ...
{% endhighlight %}

The `{}` is a cool trick that I didn't know about before reading the `gxpr` source. It means "execute all of this code as one block", meaning that if the `test` fails, none of it will execute.
{% highlight bash %}
test -z "$res" && {
    ...
}
{% endhighlight %}

If we're in this block, output that google didn't have an answer, then the original arguments. The `1>&2` means redirect all output from `stdin` to `stderr`.
{% highlight bash %}
echo "google doesn't know" "$@" 1>&2
{% endhighlight %}

We want to output a link to Wolfram Alpha, and we want the link to stand out. We use control characters (`\033[4m` to start the underline, `\033[0m` to end it) to make sure that it's underlined.
{% highlight bash %}
echo "⌘ click: \033[4m$WOLFRAM?i=$EXPR\033[0m"
{% endhighlight %}

Next, we kill the script. We exit with an error code of 1 to show that there was an error executing the request.
{% highlight bash %}
exit 1
{% endhighlight %}

And finally if we didn't make it into the block that executes when there's no result, we echo the result onto the screen.
{% highlight bash %}
echo "$res"
{% endhighlight %}

That's all, that's a line by line breakdown of how gxpr works. If you have any corrections or additions, I'd love to hear them in the comments :)
