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
echo "Third content" > content03
git add .
git commit -m "Third"
{% endhighlight %}

And meanwhile...

{% highlight text %}
cd ../isabelle
echo "More second" >> content02
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

Now Harry has an issue in that his local repository has a different history from the
official history in `shared`. Git is conservative and won't allow a "push" under these
circumstances because it wants to be able to guarantee that the remote repository is
safe and consistent.

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

Merging in Git
--------------

If you've used version control, you're used to merging other people's changes.
You make your working copy look like it should to be the "next" version, and then
you commit. We have the same goal with Git; the idea is to make the working copy
look like it should, incorporating everyone's changes.

But there is one important difference that comes from Git being distributed: we are
merging commits, not just changes to files! So in one sense, even though I
haven't discussed branches yet, it's like Harry just created a (nameless)
branch, because now his commit history differs from `isabelle` and `shared`.
(Isabelle got to `shared` first, so her commit history is the "official" one.)

Merging commits seems more complicated at first. But it's one of the most
important, powerful, and valuable features of Git. First, it allows us to commit
changes even if we're not connected to the server. Second, it's actually much safer,
because both Harry and Isabelle's changes are saved and can be accessed separately
*forever*. So if the merge is done wrong, we can more easily go back and figure
out why and fix it. 

Harry can't continue with the history in his local repository differing from
the "official" history; it's interfering with his ability to bring in work from
Isabelle. To sort things out, Git requires us to create a "merge commit". This
is a commit that combines Harry's commits with the one he downloaded from the
server. 

The "merge commit" is needed because there has to be a single accepted sequence
of commits. We are telling Git that even though Harry and Isabelle made changes
in parallel, Git should think of Harry's changes as happening "after"
Isabelle's. Another way to think of it is that Harry is promising Git that he
has taken Isabelle's work into account and made sure that both of their changes
have been incorporated correctly.

For this example, you probably noticed that I intentionally made changes that were
compatible and would not cause a conflict. As a result, we don't have to do any
conflict resolution. That particular pain is coming in the next chapter.

Because there are no conflicts, to make things as easy as possible, if Git
knows how to launch an editor for us (e.g. EDITOR environment variable) it will
open the editor with a pre-defined commit comment that explains the situation.
All Harry has to do is to save the commit comment and exit the editor.  

Successful Push
---------------

After Harry exits the editor, Git will have its one accepted sequence of commits
(see `git log` for yourself to find out what that looks like) and Harry can push
his changes to the server.

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

*****

One Side Note
-------------

There are authors who advocate the use of the Git command `rebase` in order to avoid
the use of the merge commit and simplify the change history in the log. There are times
where rebase is appropriate (for example, where a planned feature moves from one version
of a product to another). But I do not believe rebase should be used to avoid a merge
commit. In my view, the fact that two people were working in the code base at the same
time *should* be reflected in the commit history. 

Later we'll talk about feature branches, which are a much more elegant way
to deal with routine and regular merging. When a feature branch is merged back into the
"main line", there will still be a "merge commit", but only one for the whole feature. 

*****

Wrapping Up
-----------

At this point, this still looks a lot like Subversion at the surface. When I
had new folks starting out with Subversion, I used to use this (approximate)
rhyme, which I thought was clever but which never caught on:

        Still this lesson you must get;
        First you update, then you commit.

However, as I mentioned, the one really important difference is that
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

