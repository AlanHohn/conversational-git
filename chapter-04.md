---
layout: chapter
title: "Chapter 4"
description: "Simultaneous changes"
previous: "03"
next: "05"
tags: []
---

Teamwork
--------

Of course, we could continue working like the previous chapter. Harry or Isabelle
makes a change, pushes it, the other pulls it, and everyone is kept up to date. But
in a real environment, Harry and Isabelle are going to be making changes at the same
time.

Starting from the scratch directory again:

{% highlight text %}
cd harry
echo "Third content" > content-03
git add .
git commit -m "Third"
{% endhighlight %}

And meanwhile...

{% highlight text %}
cd ../isabelle
echo "More second" >> content-02
git add .
git commit -m "Second"
{% endhighlight %}

At this point, `git log` for Harry and Isabelle is different, and neither has pushed
their change to `shared`. Whoever pushes first will "win" in the sense that the other
will be responsible for dealing with the effects.

Let's say Isabelle goes first:

{% highlight text %}
git push
{% endhighlight %}

The Failed Push
---------------

Now Harry has an issue in that he has a commit in his local repository that's based on
an "old" commit as far as the `shared` repository is concerned. Git is conservative
about this kind of thing, and it won't allow a "push" under these circumstances because
the merge would have to happen in the remote respository, which would be very confusing
and might break.

So if Harry tries to push:

{% highlight text %}
cd ../harry
git push
{% endhighlight %}

Back comes a message telling Harry that the push failed. Be grateful; in modern versions
of Git there's a nice "hint" that describes the problem very clearly. Before, we got
cryptic error messages and we walked through the snow, uphill both ways, to get them.

This error isn't really that bad of a problem; Harry just has to "pull" first.

{% highlight text %}
git pull
{% endhighlight %}

The Merge Commit
----------------

For this example, you probably noticed that I intentionally made changes that were
compatible and would not cause a conflict. As a result, we don't have to do any
conflict resolution. However, Git still wants us to create a "merge commit". If it
knows how to launch an editor for us (e.g. EDITOR environment variable) it will
even bring up an editor filled in with a commit comment.

This "merge commit" is needed because there needs to be a single accepted sequence
of commits. Right now Harry's commit and Isabelle's commit pretty much happen
in parallel.  Since Isabelle won the race to push to `shared`, Harry needs to
merge in his commit so that it happens "after" Isabelle's.

All that is a long explanation for a very simple thing. Once Harry makes his merge
commit (save the editor window and exit), he is up-to-date with `shared` and
can now push the merge commit that has his changes:

{% highlight text %}
git push
{% endhighlight %}

When Isabelle does a pull, she will get Harry's new changes:

{% highlight text %}
cd ../isabelle
git pull
{% endhighlight %}

And at this point everyone's `git log` is the same again. Of course, if Isabelle had
made commits in the meantime, she would have to pull before pushing, and so on.

One Side Note
-------------

I feel compelled to say a word about "rebase", solely because some Git tutorials
encourage it at this point as a way of avoiding that "merge commit" I mentioned.
My view on "rebase" is a solid "no". For one thing, it's not like there's some
fixed supply of commits that gets used up. For another, that merge commit means
something; it means that two people were working in the code base at the same
time. It just feels like the history should reflect that.

Later we'll talk about feature branches, which are a much more elegant way
to deal with the whole merging thing. So for many Git projects this whole
"merge commit" business is not a live issue anyway.

Wrapping Up
-----------

At this point, this still looks a lot like Subversion at the surface. When I
had new folks starting out with Subversion, I used to use this rhyme, which I
thought was clever but which never caught on:

Still this lesson you must get;

First you update, then you commit.

However, there's one really important difference, and that's the fact that
Harry was able to commit code to his own repository the entire time, and was
actually merging commits, not changed files. With Subversion, there are numerous
occasions where you find out that an `svn up` is required before a commit is
allowed. 

With Subversion, that merging occurs when you have uncommitted changes in
your working tree, and there's no convenient way to store them so they don't
get damaged during the merge. (Of course, you can branch, but that's also a
server-side activity and a little bit of a pain given how frequently it
happens.)

With Git, those files are safely committed; you never have to merge into a modified
working copy.

In the next chapter, we'll look at what happens in the inevitable case where the
changes happen at the same time, to the same files.

