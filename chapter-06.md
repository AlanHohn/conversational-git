---
layout: chapter
title: "Chapter 6"
description: "Branching Out"
previous: "05"
next: "07"
tags: []
---

Yes, the chapter title puns seem to be getting worse as we go. Not really
something I'm in control of.

What Are Branches For
---------------------

One of the things I love about Subversion compared to other systems I've used
is how easy it is to branch. I've used version control systems where
"branching" occurs at the item level, which isn't really a branch at all.
The way Subversion is architected internally, a branch is cheap because it's
a shallow copy of the repository state at a point in time.

At the same time, on my last big project using Subversion, we didn't really
find ourselves branching all that much. For the most part, a branch came about
when it was time for a release, and we didn't typically commit many changes on
that branch afterwards. (I know, that sounds more like a tag, but (a)
Subversion doesn't care about "branch" versus "tag" semantic squabbles; and (b)
we did commit on the branch sometimes.)

I worked for a while on a project that used Subversion for something called
"feature branches". The whole project was organized using a ticket system, and
before you worked a ticket, you made a branch with that ticket number as the
name, worked all your changes in there, and notified someone when you were
done. It was good because the senior developers got to look over code changes
and often had good suggestions for you to implement before they agreed to merge
your change into the trunk. Peons like me didn't ever commit to trunk.

Of course, I found a way to break it. I was working on a somewhat larger issue,
and it took a little time to finish. By the time I got done, quite a bit of stuff
had changed in trunk, and I wanted to make sure my files would merge cleanly and
work. So I merged trunk into my branch, fixed some conflicts, and tested things
out. Then I blithely submitted my ticket as ready for checkout. I don't know
what the senior developer had to do to merge my changes back into trunk, but I
know that I got told not to ever do that again.

What's interesting is that this happened years before I ever used Git for the first
time, but for me it was the natural, obvious, "of course you do it that way"
workflow. Production came from trunk, so before you mess it up, you do
everything you can to make sure your changes are good changes.

This is the Git workflow! All the stuff about feature branches and pull requests is
all based on the idea that in software development, we group related changes into
features and we work those features in parallel, either as a team or individually.
Like I said before, in Git we want commits to be as small as possible, so we use
branches to group related commits into a feature so they can land in a product all
at once. 

One last Subversion note: even at the time, there was a way to do that merge and
not break things the way I did. And later versions of Subversion add merge tracking
that seems to work really well. But I think most people would agree that feature
branches are handled amazingly well in Git compared to other tools.

Doing Some Branching
--------------------

I wasted a bunch of space with that war story, so let's get to actually doing
some work in Git. Harry's been given the task of putting Shakespeare quotes into
the repository. Now that we've introduced feature branches, he starts by making
one:

{% highlight text %}
cd harry
git checkout -b shakespeare
{% endhighlight %}

It looks a little strange to use `git checkout` to make a new branch, but no stranger
than using `svn copy`. I'm showing `git checkout -b` even though there is a `git branch`
command that will do it, because `git branch` doesn't switch to the new branch,
and when you're using feature branches you don't need the hassle of remembering to switch
before you commit changes.

Note that this command is a lot different from branching in Subversion using `svn copy`.
In particular, we didn't have to specify a "remote" URL. This branch is totally local
to Harry's Git repository. If you do `git branch` in Harry's repository, you'll see it,
but if you do `git branch` in `shared` or `isabelle` you won't.

This new branch is based off the latest work that Harry pulled. Harry can do
all the work he wants here. Anything he commits will affect the `shakespeare` branch
only, and will not affect the original branch (which is called `master`).

{% highlight text %}
echo "Why man, he doth bestride the narrow world like a Colossus" > spear-01
git add .
git commit -m "Cassius"
echo "But masters, remember that I am an ass" >> spear-01
git commit -am "Dogberry"
{% endhighlight %}

At this point `git log` will show those two new commits, because Harry is still working
in the feature branch.

Keeping Up To Date
------------------

In the meantime, Isabelle has been working on other changes. She hasn't learned
about feature branches yet:

{% highlight text %}
cd ../isabelle
echo "This is no ordinary line" > content-04
git add .
git commit -m "Fourth"
git push
{% endhighlight %}

Isabelle pushed her changes, and Harry wants them. However, he doesn't want them
in his feature branch; that branch is just for Shakespeare. So he switches back to
`master`:

{% highlight text %}
cd ../harry
git checkout master
git pull
{% endhighlight %}

If you're following along, you'll see that Git reports this as a
"fast-forward". No merge is taking place here. Also, you'll notice that `git
log` does not show our two Shakespeare related commits in the log, and the new
`spear-01` file is not in the working copy.

Merging a Feature
-----------------

Assuming Harry is done with Shakespeare for now, he'll merge those changes in from
the feature branch. He's already in `master`, which is where he wants to be to
merge in changes.

{% highlight text %}
git merge shakespeare
{% endhighlight %}

Git brings up the editor to let us make a merge commit, and once we save and exit the
editor window, the change happens. All the stuff I said previously about handling
conflicts, aborting merges, all that stuff applies here as well.

Now that the changes are merged, we can push them:

{% highlight text %}
git push
cd ../isabelle
git pull
cd ..
{% endhighlight %}

Now Isabelle has the changes too. She does not, however, have a copy of the
`shakespeare` branch; she only gets the new file `spear-01` because it was
merged into `master`. (She does have a `master` branch, of course. It got created
when we cloned the `shared` repository for her.)

Wrapping Up
-----------

This was a really basic feature branch, and I didn't show most of the best reasons
why you might want to use one. Fortunately I'm using the natural numbers for these
chapters, so I'm not likely to run out.

Even though this was a really basic feature branch, I don't want anyone to lose
sight of what we did here. Without ever using a remote server at all, we created
a branch, committed some changes to it, and merged it back into the main line.
Even that basic capability is enough to change the way that a developer works
when they're working multiple tasks at the same time (which of course is most
of the time). A feature branch represents freedom from worrying about leaving
the codebase in a broken state while you're implementing something complex or
risky. It also provides a quick way to context switch when you're working
multiple things. These benefits of feature branches exist whether or not you're
using Git, but the ability to make a feature branch while working disconnected is not
something to be taken lightly.

