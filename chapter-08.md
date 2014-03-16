---
layout: chapter
title: "Chapter 8"
description: "Conflicts are a Feature, Not a Bug"
previous: "07"
next: "09"
tags: []
---

Feature Branches with Conflicts
-------------------------------

Straight to the Git this time.

First let's make sure we're clean.

{% highlight text %}
cd harry
git checkout master
git pull
cd ../isabelle
git checkout master
git pull
{% endhighlight %}

Isabelle has some work to do and makes a feature branch.

{% highlight text %}
git checkout -b hamlet
echo "Nymph, in your orisons. Be all my sins remember'd" > spear03
git add .
git commit -m "Hamlet and Ophelia"
git push --set-upstream origin hamlet
{% endhighlight %}

And meanwhile Harry is working too.

{% highlight text %}
cd ../harry
git checkout -b julius
echo "The fault, dear Brutus, is not in our stars" > spear03
git add .
git commit -m "Cassius and Brutus"
git push --set-upstream origin julius
{% endhighlight %}

Even though they both created the same files, both are allowed to push to the
`shared` repository because their changes are on feature branches. This is another
important point about feature branches that makes them worth the trouble, even
when the change is relatively small. They allow you to choose *when* to incur the
pain of merging, and in the meantime allow you to work in peace.

Merging Feature Branches with Conflicts 
---------------------------------------

Of course, at some point someone is going to have to merge this. Let's make Harry
do it, since he's so smug about how well he knows his Shakespeare.

{% highlight text %}
git checkout master
git pull
git merge origin/hamlet
{% endhighlight %}

So far so good; everything merges without incident. You'll notice that the syntax
changed a little; this is Git forcing us to be explicit. Because Harry's never
worked with the `hamlet` branch, he doesn't really have a local `hamlet` branch,
he just has the one he downloaded from `shared`. If you were to just say
`git merge hamlet` Git provides a hint letting you know what to do.

Harry now has to merge in his own changes from `julius`:

{% highlight text %}
git merge julius
{% endhighlight %}

That doesn't work, because it's trying to add a file that was added in a different
commit, so we have a conflict.

Clean Merging Is Clean Living
-----------------------------

At this point Harry could do the same thing we did last time with conflicts; just edit
the conflicting file, make it look like it should, and do the commit. For this toy
example, where we're doing everything from the command line, that would be fine.

But in the real world, there are a couple issues with doing it that way. First, the
change will be going directly into `master` without being tested. Second, most of the
time, the person merging the feature branch into `master` has reviewed the code, but
they're not the author of it. It really should be the author of the feature branch who
makes sure things are merged in the right way. Finally, for most projects using Git,
merging into `master` is done automatically when someone has reviewed the change
on a website and clicked "Merge". That's one of the primary purposes for tools like
GitHub; to improve code review by reviewing smaller bits at a time, making review easy,
and making it a part of the normal lifecycle of development.

The point is that when we're merging into `master`, the merge should be clean, because
that increases the chance that we're going to get the change that we want. So instead
of fixing the conflict in `master`, Harry is going to fix the conflict in the feature
branch. This works in Git because Git does a *really* good job of keeping track of
what commits have been merged into what branch.

{% highlight text %}
git merge --abort
git checkout julius
git merge master
{% endhighlight %}

We get the same conflict, but from the other side -- now we're merging the Hamlet
quote into the feature branch. 

After Harry fixes the conflict:

{% highlight text %}
git commit -am "Hamlet and Cassius"
git push
{% endhighlight %}

We choose to do a `git push` here because we updated the feature branch and want that
update to be availble. Later, when we talk about how sites like GitHub work with this
flow, this will be important.

{% highlight text %}
git checkout master
git merge julius
git push
{% endhighlight %}

Not only does Git do the merge correctly, but it even recognizes it as a "fast-forward".
When we fixed the conflict in the branch, we made a merge commit, and that commit is
"after" the commit that merged the Hamlet quote into `master`. If that sentence didn't 
make sense, it's because I didn't explain it well; we're getting into DAG territory
and I'd rather focus on effects than architecture. The important thing is that
this stuff *just works* and we can rely on it.

Wrapping Up
-----------

At this point, I've introduced the basic workflow for Git, including feature branches.
There are lots more Git commands, and I probably need a few more chapters to cover
some other things, but the vast majority of the time these commands are the
only Git commands I use (including `git log` and `git status` that I mentioned earlier).

Is the Git approach more complex in some ways than similar functions with Subversion?
Definitely. In some ways they're very similar; if you use feature branches, you have to
think about when to make them and when to merge them, and you have to remember what branch
you're on before you commit code. But Git has the extra steps of committing to a local
repository.

The benefit you get for that, in addition to being able to work disconnected, is that
merging becomes much more about combining commits rather than combining changes into
files in the working copy. Not only is that safer and easier to abort, it makes it
easier for Git to track what's been merged so you can merge from any angle and get
intelligent results.

Next chapter we'll talk about what happens when it all goes wrong; recovering from
mistakes.

