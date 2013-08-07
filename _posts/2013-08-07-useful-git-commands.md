---
layout: post
title: Useful git commands
date: 2013-08-07
---

Git's a fantastic tool. If you know what you're doing, you can pretty much do anything that you can dream of.

However, to get 90% of the way there, you only need to know a handful of commands.

### git add/commit/push/pull
I'm not going to cover these as you can find information about them everywhere. add/commit/push/pull are the cornerstones of `git`, so you need to understand them to do anything else.

### git checkout <file> <rev>
Revert a file to a certain point in time. If you specify a revision (this can be a commit hash, a tag name or a branch name) it will show the file as it looked at that commit.

My main use case however, is when I've made lots of changes to various files (generally whilst debugging) then I want to discard them all. It's as simple as typing:

{% highlight bash %}
git checkout .
{% endhighlight %}

### git reset <file>
Sometimes, I stage a file ready for committing, only to realise that I don't want to commit it with all the others. To remove a file from the index, you can use:

{% highlight bash %}
git reset <file>
{% endhighlight %}

### git add -p <file>

`git add -p <file>` allows you to step through each block of changed text in the specified file and say yes/no to staging that chunk for commit. This is especially useful when you fix multiple things in the same file that aren't really related and want your commits to be logically separated.

### git stash
I regularly make lots of changes in a branch before realising that I want to put that development on hold and work on something else. Instead of committing half finished features, I use `git stash`. `git stash` is shorthand for `git stash save <description>`.

Once you've stashed changes you can do whatever work you need to do before going back to your work in progress branch and using `git stash pop` to apply the changes to your working tree again.

`git stash` is also very useful when you're merging another branch into your current one and `git` won't let you as your uncommitted changes will cause a conflict. `git stash` then merge and reapply your changes to force the merge conflict to happen.

### git patch

Finally, `git patch`. Sometimes, the history of a branch gets so messy that you don't want to mess around with `git rebase` or anything like that. You just want to take the differences between your feature branch and develop and apply them as a single commit. `git patch` is perfect for this.

To generate a patch, use the following command:

{% highlight bash %}
git format-patch <branch_to_compare> --stdout > your_changes.patch
{% endhighlight %}

Then, checkout your base branch and apply it. I prefer `git am` for this over `git apply` as it creates a commit when applying the patch.

{% highlight bash %}
git am --signoff < your_changes.patch
{% endhighlight %}

Whilst that seems like a trivial example, patches can be used in a lot of situations. For example, we had a feature that was developed against our develop branch, but develop wasn't as stable as we wanted and couldn't be deployed. This feature was a fairly important release, so we generated a patch against develop to see only the data that we changed, then applied it to the master branch as a hotfix and deployed master.
