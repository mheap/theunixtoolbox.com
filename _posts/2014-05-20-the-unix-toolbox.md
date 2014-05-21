---
title: The Unix Toolbox
layout: post
date: 2014-05-20
---

_This article originally appeared in the December 2013 issue of [PHP Architect](http://www.phparch.com/)_

> The beauty of a Unix-based operating system is that it has a multitude of useful tools that most people don't know about. You can use them on their own, chain them together, you can do almost anything you can think of.

We'll take a look at some popular tools and some lesser known gems. As well as tools, we'll cover tips and tweaks for making your command line do all the work so that you don't have to. From search and replace to installing software without having to find it first, the command line can do it all!

## The Unix Philosophy
The thing about Unix is that you can use as much or as little as you need. I think Doug McIlroy said it best:

> This is the Unix philosophy: Write programs that do one thing and do it well. Write programs to work together. Write programs to handle text streams, because that is a universal interface.

That philosophy of making small apps that do one thing (well), and making them accept textual input from almost anywhere is what makes Unix great.

## Disclaimer
When I say Unix, what I actually mean is \*nix. Linux, OSX, FreeBSD and more. Although development on these projects has diverged, they all still come from the same Unix core. This means that everything I'm going to talk about should at least be possible on your chosen OS (unfortunately, that doesn't include Windows without quite a bit of work).

## The basics
When we think of the command line, most people think of screens full of text that don't make any sense, about cryptic commands that take longer to learn than a spoken language.

Whilst that can be the case for some, a lot of the commands aren't like that. Most are contractions of the action that they perform which, once you know what the expanded version is, makes them much easier to remember.

### Hands on

Let's start by taking a look at the five "must have" utilities. Feel free to bring up a terminal and try the commands out as we go through them. If you're already comfortable with the basics, feel free to skip to "Making them do more" for the juicy bits.

#### ls
`ls` is a utility that's been around since 1971 - a pretty long time. Whilst it seems like an oddly named utility, it's actually short for "list" - the function it performs. 

Running `ls` from the command line will show a list of everything that's visible in that directory. That's all it does, it lists a directory's contents.

#### mkdir
`mkdir` is another important one. Again, it sounds like a random selection of letters, but once you know that it stands for "make directory" it starts to make a lot more sense.

So, let's make a directory. `mkdir` has one required parameter, the directory to create. Let's create a directory called "phparch" with `mkdir phparch`. If everything was successful, you should just see your prompt again - if anything went wrong it should show you an error message.

We can check if the directory we just created is there by using `ls` and looking at the output.

#### cd
Now that we have a directory, let's do something with it. We need to change our current working directory to be our new directory. To do this we use `cd`, which stands for (guessed it yet?) "change directory".

So, we can use `cd phparch` to change directory into our new directory, then type `ls` again to see what's in there. As we've only just created it and not put anything inside it yet, there should be nothing shown on screen.

### Half way

We're half way through the essential commands, and hopefully we can see how even with just three commands, we can do things much more efficiently than we can using a GUI. Now that we can create folders and move into them, let's take a look at creating some text files to store some notes.

#### vim / emacs / nano

Up until now, our commands have been quite closely named to the function that they perform. Unfortunately, that rule gets thrown out of the window when it comes to text editors. 

The three main editors on the command line are vim, emacs and nano. It doesn't matter which you use (I prefer vim, personally), but you're probably going to end up using one. For now, I'd recommend using `nano` as it's the easiest of the bunch.

So, let's create a text file in our new "phparch" folder. To do this, you just need to type `nano <filename>`. In this case, we're going to create a file called "owners.txt", so we type `nano owners.txt` and hit Enter.

The window should change, your prompt should disappear and your cursor should be at the top of an empty window. This is nano, the text editor. Type "musketeers.me" into the file, then we need to save your text and exit nano. 

This is where it gets tricky and feels a bit more like black magic. The commands to save and exit are documented at the bottom of the screen, in a roundabout way. You might notice that there's something that says "^O Save". This means press "Ctrl+o" to save the document. Give it a go hitting Enter when prompted.

Next, we need to quit nano. You can find the command to quit at the bottom of the screen too (hint: It's ^X), so press that and you should be back on your command line. We can see if the file was created by typing `ls`. You should see something called "owners.txt". If we wanted to edit that, we simply type "nano owners.txt" again. All your existing text should be there, and you can edit it as you see fit. For now though, just press "^X" to exit nano again.

#### grep
Grep is probably my favourite utility. It stands for **g**lobal **r**egular **e**xpression **p**rint.

In it's most basic form, it's used for searching through files in the current directory. Imagine that instead of one text file, we had 100 in this directory. Now, imagine that we need to find out which one of those 100 contained the word "musketeers". Instead of using `nano` to open them all up one by one, we can use grep.

The easiest way to use grep is to run `grep "musketeers" *`. This means search for "musketeers" in everything in the current folder. It's important to note that it's case sensitive, and will only look at files in the current directory. We can change that behaviour, but we'll come to that later on.

So, running `grep "musketeers" *` should return one line, containing the name of the file that the string was located in, and the entire line that it was located on for context.

#### rm

Finally, we want to delete the file we just created. Thankfully, we're back to the names of commands being easy to guess. `rm` is short for "remove". 

Type `ls` to make sure that our file is there, then type `rm owners.txt` before running `ls` again. Hopefully you'll notice that whilst owners.txt was there the first time that you ran `ls`, it had gone the second time. Congratulations, you just deleted a file!

### Recap
Ok, you got me, that was six commands. Whilst `grep` isn't one of the commands that you need to know to use the command line, it's far too useful for us to leave it out.

With those six commands, you should be able to hold your own on the command line, creating directories and editing text files, removing anything else you don't need.

But surely there's more you can do, right? You can do all this stuff just as quickly, if not quicker, with a GUI! Well, you're right, they *can* do more. Let's take a look at just what they can do.

## Making them do more

All of these utilities doing one thing each is great, but if they just did one thing in one way, we'd end up with hundreds of utilities that do very similar things. To get around this, we use the concept of "flags".

You remember that the `ls` command is used to show the contents of a folder? Try running `ls -l` - you should see a list of all of the files in a folder, along with loads more information (which we don't need to worry about for now). We've done the same core action, listing the contents of a directory, but we've formatted it in a different way. There's loads of flags available for most commands. You can see a complete list of supported flags by reading the manual page for each command. Type `man command` to show the help page e.g. `man ls` to get help with the `ls` command.

Some commands take arguments for their flags. A good example of this is `grep -C`. The -C flag means "context", and the argument it takes is the number of lines to show either side of the line that matched. So, `grep -C 5 "foo" *` would show 5 lines either side of any line that matches "foo" within files in the current directory. This is shown in the man page as `-C[num]`, meaning that the -C flag takes a parameter.

Finally, you can generally combine flags into one parameter if you don't need to provide a value. e.g. `ls -a -l` is the same as `ls -al`, but `grep -iC 5` won't work as `-C` requires a value.

## Chaining them together

Now that we're comfortable with making the commands do what we want, wouldn't it be awesome if we could start using them in conjunction with each other?

#### Pipes

To do this, we use something called a pipe (The `|` character). At the beginning of this article, I mentioned that these commands can get their input from anywhere. Pipes allow you to redirect the output of one command and use it as the input of another one.

For example, to find all of the items in a folder that I edited in December, I can run `ls -l | grep "Dec"`, which will show the long output version of `ls` and search the output for the word "Dec", returning only lines that match.

That's a pretty basic example, so let me show you a few more examples that are *really* useful, but use commands that you've not come across yet:

##### Show all files in a folder, sorted by size
To do this, we use `du` (disk usage), specify a max depth of one (`-d1`) and the pass this into `sort -rn`, saying sort this output in reverse, sorting the strings as though they're numbers and not strings (e.g. 1, 2, 3, 10 not 1, 10, 2, 3).

{% highlight bash %}
du -d1 | sort -rn
{% endhighlight %}

##### Show every line of a file, removing any lines that contain the word "bob".

To do this we use `cat`, which outputs a file's contents, and `grep` which searches a text stream. We provide the `-v` flag which means "invert match", making it select the non-matching lines.

{% highlight bash %}
cat afile.txt | grep -v "bob"
{% endhighlight %}

#### xargs and redirection

As well as pipes, we have two other tools in our chaining arsenal. The first is `xargs`. `xargs` allows you to pipe output from one command and use each new line as an input parameter to another command.

For example, to delete everything containing ".git" in the current path:

{% highlight bash %}
find . -name "*.git*" | xargs rm -r
{% endhighlight %}

This would find everything whose name contains ".git" and output it one item per line. Then each line will be passed in to `rm -r`, meaning the file is removed.

If you wanted to curate the list instead of blindly passing it into xargs, you could use file redirection to store a temporary list: 

{% highlight bash %}
find . -name "*.git*" > findgit.txt
{% endhighlight %}

This would create a file called "findgit.txt" which you could open, read and edit as you see fit. Once you're done, save it and then you can use it as the input file for `rm` by using `cat` and `xargs`.

{% highlight bash %}
cat findgit.txt | xargs rm -r
{% endhighlight %}

Running that command would delete everything from the paths specified in the file "findgit.txt"

Another real world example of how xargs can be useful is by doing a find and replace across all files that specify a certain criteria. For example, I want to rename the class "User" to be called "Account".

First, we start by finding all files in the current directory that contain "new User":

{% highlight bash %}
grep -r "new User" *
{% endhighlight %}

Next, we can test our substitution by piping it into `sed`. `sed` is a utility that is used for manipulating text. In this instance, we're going to use it's string replace functionality.

{% highlight bash %}
grep -r "new User" * | sed 's/new User/new Account/g'
{% endhighlight %}

We should see the output we saw last time, but instead of it saying "new User" it should say "new Account".

Once we're happy that the replace works, we add a few more arguments in. By passing the `-l` flag to `grep`, we say "give me only the filenames", and by passing the `-i` flag to `sed` we say "make the change in place". So, our command becomes:

{% highlight bash %}
grep -lr "new User" * | xargs sed -i s/new User/new Account/g'
{% endhighlight %}

After running that command, all your files that used to use the "User" class should now use the "Account" class.

#### Bash

Sometimes, feeding files blindly into another expression using `xargs` just isn't good enough. Fortunately, we have the `bash` programming language to help us do some more complicated things. 

We're probably going to jump forward quite a lot here to keep things as short as possible. If I use a command or a flag that you want to know more about, remember that you can type `man command` to find out more.

##### Removing empty directories

Bear with me whilst I set the scene here. Imagine that we have a directory that has 10 folders in it. In each of those folders, we have another 10 folders. Then, in each of those we have 10 more. Inside an indeterminate number of those folders, at any level, there may be a file named "foobar.txt".

Now imagine that we want to delete all of the empty directories in there, but we don't know which of them contain "foobar.txt". Instead of checking them all one by one, we can use `bash` to construct a little program to do it for us.

I'm going to use the concept of subshells here, which are shown using the `$(command)` syntax. The command provided is run in another shell, and the data returned by the command is passed through to our current process.

So, we start off by finding all of the folders below our current directory and saving that list:

{% highlight bash %}
SEARCH_DIRS=$(find . -type d)
{% endhighlight %}

Then, we can loop through all of those directories and look at their contents.

{% highlight bash %}
for DIR in $SEARCH_DIRS; do
  ls $DIR
done
{% endhighlight %}

This will output the contents of every directory in our list. Now, we want to check which of them are empty and remove them. To do this, we use `test`.

{% highlight bash %}
for DIR in $SEARCH_DIRS; do
if [[ -z "$(ls $DIR)" ]]; then
  rmdir $DIR
fi
done
{% endhighlight %}

`[[ -expr ]]` is a commonly used alias for the `test` command. `-z` is a check that looks to see if the string provided is empty. As we're providing the output of `ls $DIR` as the string, if the folder is empty the string should be too.

Finally, if the test is true (i.e. the folder is empty) we remove the directory.

## Useful flags

As we've seen, you can change the behaviour of most utilities by providing flags when you're calling it. There are a few useful flags that are common across a lot of utilities. To check what flags are available for a specific command, run `man command`.

The ones that I've found myself most commonly using are as follows:

* -i (Case insensitive e.g. `grep -i string *`)
* -r/-R (Recursive e.g. `ls -R Downloads`, `grep -r string *`)
* -v/-V (verbose or version, depending on the utility)
* -h (human readable file sizes, e.g. `du -h`, `ls -lh`)

## Config options
As well as passing flags to utilities, you can configure a lot of them to have smart defaults. This is done using either a .rc file, or an environment variable.

#### .rc files
.rc files generally live in your home directory, and change the way that programs work. They're loaded automatically whenever you use a utility that is looking for that config file.

They could be as simple as adding "startup_message off" to `.screenrc` to disable the startup message, or as complex as a 350 line `.vimrc` that configures everything from the font size to what happens when you press an arbitrary set of keys.

#### Environment options
Other utilities are controlled by environment variables. The best example I have is `grep`, which is controlled by `GREP_OPTIONS`.

Unlike .rc files, they're not loaded automatically. They need to be added to a file that is loaded each time a terminal is launched such as ".bashrc" (or ".zshrc"/other depending on your shell). 

When searching with `grep`, I usually want to ignore vendor folders when programming. By adding `GREP_OPTIONS='--exclude-dir=node_modules'` to my .bashrc file, it means that grep will never search inside "node_modules" folder unless I run grep as `GREP_OPTIONS="" grep -r "string" *`, setting GREP_OPTIONS to be empty for this request only.

## More utilities

The command line doesn't stop with the built in functions. Every day, people release new utilities and frameworks to make our life easier. Here are a few of my favourites:

##### GRC

GRC is short for the "generic coloriser". It's a utility that allows you to pipe output through it and have it apply colours via user defined stylesheets. The patterns it matches are specified via regular expressions, and it ships with a lot of useful stylesheets.

##### zsh-syntax-highlighting
This one's ZSH specific I'm afraid, but it's too good to leave out.

Again, this utility adds colour to your terminal via user defined rules. Out of the box, it does things like:

* Highlights strings inside commands
* Highlights matching brackets
* If the command you're trying to use doesn't exist, it highlights it in red. Otherwise, it's shown in green.

In addition, I added one of the suggested rules that makes the prompt's background red and turns the text white every time I type the characters `rm -rf`. That visual indicator is a good reminder to double check the command before I hit enter.

#### gxpr
Finally, let's take a look at `gxpr`, a script that allows us to search google calculator and outputs the answer to the terminal.  This is a perfect example of why Unix is great. Someone decided that they used Google Calculator enough that they didn't want to use a browser each time, they wanted to do it all from the command line.

So, they set out researching how to do it and glued together a few `curl` commands with some perl and they had a working solution in less than 20 lines of code.

If you're interested, you can find the script at <http://bit.ly/12CFVRj>

## Conclusion
Hopefully that wasn't too scary as an introduction to Unix. Next time you need to solve a problem, instead of running to Google to find a GUI to do it, try searching for a command line utility. Each time you learn a new one, it will magically become part of your day to day usage. Before you know it, you'll be spending most of your time on the command line, and you'll wonder how you ever lived without it.


