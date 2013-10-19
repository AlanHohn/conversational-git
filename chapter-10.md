---
layout: chapter
title: "Chapter 10"
description: "Serious Issues"
previous: "09"
next: "11"
tags: []
---

What Did I Just Do?
-------------------

The one consistent thing about mistakes is that realization happens about 100ms
too late. Whether it's committing to the wrong branch, merging to the wrong
branch, or some other problem, "messing up" your repository is the worst feeling.

The most important thing with Git is when this happens, don't panic, and don't
push. Anything can be fixed, but it'll be fixed a lot easier if it hasn't been
pushed yet.

Committing to the Wrong Branch
------------------------------

When you're switching around between `master` and feature branches, it's easy
to commit to the wrong branch. Environments like Eclipse will tell you what
branch a given repository is on, but that doesn't mean it doesn't happen.

If you just need to redo a commit, with a different branch as target, it's
pretty easy.

{% highlight text %}
cd harry
git checkout -b much-ado
echo "Were she other than as she is, she were unhandsome" >> spear-02
git add .
git commit -m "Benedick"
git checkout master
echo "But being no other but as she is, I do not like her" >> spear-03
git commit -am "Benedick continues"
{% endhighlight %}

We switched back to `master` probably for some good reason. Then we forgot
we switched and went back to making commits that belong on our feature
branch.

In this case, we don't want that commit to apply to `master` at all. We need
to rewind `master` to the point before that commit, but in a way that keeps
the change in our working copy so we can apply it to the branch.

{% highlight text %}
git reset --soft HEAD^
{% endhighlight %}

We used `reset` rather than `checkout` this time. Last time we were content to
just make a new commit after undoing the bad stuff. This time we want that commit
to have *never happened*, because it would be confusing to people to see a
"Much Ado" commit on `master` before that feature branch got merged in.

We also used `--soft`, which wasn't strictly necessary, but it's a nice touch
because it leaves our "staged" changes. This means we can redo the commit without
having to worry about doing `git add` or `git commit -a`. Don't worry about
this kind of touch while you're learning Git; it's the kind of thing that
comes naturally over time. The more mistakes you make, the faster you get to
learn the different ways to fix them.

One other thing: here I used yet another way to refer to "the commit just
before the last one".  It's exactly the same as `HEAD@{1}` but I wanted to show
you both because you'll see them both.

Now that we've backed out the commit, we can switch branches and commit it where it
belongs.

{% highlight text %}
git checkout much-ado
git commit -m "Benedick continues"
{% endhighlight %}

Merging in Traffic
------------------

You probably don't care, but what happened to the original "Benedick continues"
commit, the one that we committed to `master` and then backed out? It didn't go
anywhere. Really, all we did was just change the history for `master` so it no
longer included that commit. That commit is still floating out there but it
no longer belongs to any branch.

That's another one of those "architecture versus effect" questions. In my opinion,
we should behave as if that commit is gone, because while we could use some
esoteric command to retrieve it, there's just no value in doing so.

Similarly, what happens if we merge a bunch of commits when we didn't mean to?
We do exactly the same thing to fix it.

Let's say that Harry thinks he wants to merge his feature branch into `master`:

{% highlight text %}
git checkout master
git merge much-ado
{% endhighlight %}

At this point the marketing guy shows up and tells him that the feature needs to
wait until version 2.0 because they plan a price increase then and need to justify
it. Harry needs to get that feature out of `master`. He doesn't remember how many
commits were in that feature branch, so he does `git log` and finds the last
"good" commit. In my repository, that's `391590ed0605807042eb0dbd0eb9054396a5ec1a`;
you'll have to look up your own.

{% highlight text %}
git reset --hard 391590
{% endhighlight %}

It makes sense to use `--hard` here because we want Git to also update the working
copy. It's safe because those commits are stil available on the feature branch. In
fact, if the marketing guy were to show up and say he just remembered that he promised
that feature in the next release after all, Harry could just `git merge much-ado`
again and everything would be back where it was a moment ago.

What If I Pushed?
-----------------

That example worked because Harry had not pushed the change to `shared` yet. But that's
not very realistic. I still maintain that the best solution is just to get the files
back to the right state and make a new commit. But in some particular, probably rare
situations, that might not be preferred. What about cases where someone committed
personal information to the repository? It's not OK to just leave that sitting around
in an old version.

Let's re-merge in `much-ado` so we have something to back out:

{% highlight text %}
git merge much-ado
{% endhighlight %}

Same as before, we start by fixing our own repository first.

{% highlight text %}
git reset --hard 391590
{% endhighlight %}

Now we need to push this change to `shared`, but it's not a regular fast-forward
any more, so Git will reject it. This is one case where it's justified to do a
"forced" push:

{% highlight text %}
git push -f
{% endhighlight %}

You can believe me that the right thing happened, or you can `cd` over to Isabelle's
repository, make sure `master` is checked out, and `git pull`. The latest "Much Ado"
commits won't be brought in and they won't appear in the log.

Wrapping Up
-----------

The stuff in this chapter is the Git equivalent of surgery, and it should be as
rare as surgery. Even though the commands are short, these changes were
relatively complicated to envision. However, these kinds of fixes are complicated
in any version control software, and they'd be practically impossible in some tools
I've used. Git lets you do this, but as I said before, it doesn't mean you should.

Those familiar with Git will notice that I stayed away from `rebase` in this whole
discussion. Rebase and its merge companion `cherry-pick` would have let us choose
to keep some commits from history (or some commits from a merge) and skip others. They
also allow editing commits way back in time, or editing commits as they're merged in.

However, I also recognize that most teams using Git have that one person who gets into
the details of the tool and learns the magic. On some teams, with some tools, I've been
that person. I also know that most people are not that person, and it seems silly to
me to pretend that someone should learn how to `rebase` or `cherry-pick` to use Git.

