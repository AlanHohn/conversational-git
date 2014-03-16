---
layout: chapter
title: "Chapter 7"
description: "Remote Branches"
previous: "06"
next: "08"
tags: []
---

I was under self-imposed pressure to make a chapter title pun, so of course I
couldn't think of one.

Better Feature Branches
-----------------------

There are a couple of flaws with what we did last time with feature branches. First,
one of the benefits of version control is collaboration with others, but Harry
selfishly kept his branch to himself and only shared his changes when he was done.
Second, while Harry could commit changes to his feature branch, it only lived in
his repository, which means if his computer died, his work would be gone.

This time, Harry is going to do some more Shakespeare quotes, but Isabelle is
going to help. We still don't want the changes to hit the main line until they're
ready, so they're going to work together on a feature branch.

Harry merged the previous changes in his `shakespeare` branch, but the branch
itself is still around and he can just switch to it. (Different teams have
different rules about keeping old feature branches around.)

{% highlight text %}
cd harry
git checkout shakespeare
echo "Can the world buy such a jewel?" > spear02
git add .
git commit -m "Claudio"
{% endhighlight %}

This is the same as what we did last time, but at this point, Harry wants to
back up his change and also let Isabelle work with him. 

Pushing Feature Branches
------------------------

Here we need to talk for a minute about how pushing works in Git. Up until now,
we've gotten away with just saying `git push` because Git knew what we meant.
It knew we wanted to push from `harry` or `isabelle` to `shared`, because that's
where we cloned from when we made those repositories in the first place.

Because Git is fully distributed, it needs to allow for the cases where a repository
has *lots* of different upstream repositories. If we were helping to develop Linux,
we might have branches for Linus' main line, the current Red Hat main line, and
many others. When we make a new branch, we need to tell Git which upstream
repository this branch belongs to.

If while we're switched over to `shakespeare`, we do "git push", Git will tell us
what I just said, but shorter. It will also tell us how to fix it. When we clone
from a repository, that repository is automatically called "origin". So the first
time we push a new feature branch, we have a long command:

{% highlight text %}
git push --set-upstream origin shakespeare
{% endhighlight %}

This tells Git to send the branch to "origin", which is the label Git uses to refer
to `shared`. It tells it that the feature branch should be called `shakespeare` on
"origin" as well. From this point forward, regular `git push` will work fine.

The `shakespeare` branch exists on `shared` now, and we can pull changes to it
just like we did before with our master branch.

Isabelle can now start helping Harry with Shakespeare:

{% highlight text %}
cd ../isabelle
git pull
{% endhighlight %}

This is all that Isabelle has to do to get any new feature branches that have been
pushed to `shared`. She can now switch to that branch and Git will be smart enough
to make a local version of it:

{% highlight text %}
git checkout shakespeare
echo "Yea, and a case to put it into" >> spear02
git commit -am "Benedick"
git push
{% endhighlight %}

Note that because Isabelle got her branch from "origin", Git already knows where
to push it.

Either Harry or Isabelle can merge the changes back into master when they're
done adding Shakespeare quotes:

{% highlight text %}
git checkout master
git merge shakespeare
git push
cd ../harry
git pull
{% endhighlight %}

Wrapping Up
-----------

It's not immediately apparent, but Git did something a little clever here. In the
last chapter, we merged a couple Harry Shakespeare commits. (Harry is Bill
Shakespeare's direct patrilinear descendent, but don't ask him about it,
because then he won't shut up.)

In this chapter, we used the same feature branch to make a couple new commits
and then merged them. Because Git stores the parent of each commit, it can walk
back through that history and notice that some of the commits from the
`shakespeare` branch have already been merged. It doesn't try to merge those again,
which is good because it would find spurious "conflicts". (Before Subversion had
merge tracking it was painfully easy to make it create those spurious conflicts.)

We've still got at least one more chapter on feature branches, because we've
been working again with the "happy path" where no one gets in another person's way.
We need to look at more realistic cases.

