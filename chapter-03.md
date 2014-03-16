---
layout: chapter
title: "Chapter 3"
description: "Clone, Push and Pull"
previous: "02"
next: "04"
tags: []
---

Multiple Repositories
---------------------

In Subversion, the only way we work with multiple repositories is through
a mirror, which has to have exactly the same history to work correctly. Git
is designed around multiple repositories; to help work on a project, you
have to have not just a working copy but a "clone" of the whole repository so
you can commit, push, and pull.

Like Subversion, a lot of teams using Git share a common repository located on
some server. Rather than sending changes around to each other, which isn't efficient,
they instead send all their changes to a server and pull everyone's changes from the
server at once. 

I'm not going to spend time talking about managing repositories on a server. It
brings in a lot of issues with permissions, network accessibility, and those kinds of
things. All I will say is that if you're in the position of setting up Git for a team,
and you're responsible for managing the server repositories, do it through a tool like
GitHub, Gitlab, or Atlassian Stash. The extra features you get, and the workflow it
enables, are worth it, and it reduces the administrative burden of hosting Git. All
three of those tools can be run on your own server and your repositories can be as
private as you need them to be.

Anyway, to work with multiple repositories, Git doesn't care where they are as long
as it can get to them. Directories on a file system work just fine.

Bare Repositories
-----------------

One thing I am forced to talk about is a "bare" repository. This is just a
repository with no working copy. We're going to be using "push" to send commits
to a "remote" repository, and Git wants to make sure that we don't mess up the
remote repository's working copy by doing that. So by default, it's going to
reject any push that isn't targeted at a bare repository. Server repositories
are *always* bare, so you won't see this issue in real life. 

I could make you create a new bare repository, but then we wouldn't have the
stuff from last chapter. Instead, a quick workaround; we'll make a bare
repository based on the non-bare one we created last time. This workaround also
teaches an important command, `clone`.

Assuming we start back in our scratch directory (so if we're still in `repo`,
we need to do `cd ..`), do this: 

{% highlight text %}
git clone --bare repo shared
{% endhighlight %}

From here on out I'll ignore `repo` and just talk about `shared`. Keep it around,
though; we may do something neat with it later in the book.

Cloning
-------

Last time we started with a brand new project, but only one person gets to make that
first repository. Everyone else needs to clone it. This is important because it gets
the whole history of changes that have been made, so when we make new changes and
commit them, other people can apply them easily.

We'll assume that now there are going to be two imaginary friends helping us on
this project; call them Harry and Isabelle. They each need their own space to
work.

{% highlight text %}
git clone shared harry
git clone shared isabelle
{% endhighlight %}

`shared` is the "bare" repository we made a minute ago, and it has all the work we
did in the last chapter. With these commands, we're telling Git to copy that
repository to a new one called `harry` (and another new one called `isabelle`).
You can see for yourself that each of these new directories has a `.git`
directory and each directory has the latest content in its working copy.

Each of these new repositories is a self-contained copy of the original repository. It
points at its source, but it doesn't need to reach back to get access to any
history, any previous versions of files, or any branches. If we do `git log` in
`shared`, `harry`, or `isabelle`, we'll see exactly the same history, and `git
log` is pulling this from each local `.git` directory.

In fact, we could now throw away the `shared` directory and continue
committing to one or the other of these new repositories (and even pull content
between them directly). But to be more realistic, we'll pretend that `shared` is
the single shared repository that both Harry and Isabelle can see.

One other note: since I mentioned `git log`, which as expected shows history, I
should also mention `git status`. For the following chapters, you won't see
`git status` in the Git command stream because it would have broken up the flow
of what I'm showing. That doesn't mean it's not important. It tells you all
kinds of useful things, including what files you have that need to be committed
and where you stand with respect to other repositories that you're talking to.

Sharing Commits
---------------

Harry wants to make some changes:

{% highlight text %}
cd harry
echo "Adding some more content" >> content01
echo "Second content" > content02
git add .
git commit -m "Harry content"
{% endhighlight %}

At this point, `git log` in `harry` will look different from `git log` in `shared` or
`isabelle`. Not only the working copy in Harry's directory, but his repository also
is different from the others. In order for Isabelle to see Harry's changes, he could
send her the commit directly (via an email with a patch or something), but it's much
better for him to just push it to their shared repository.

{% highlight text %}
git push
{% endhighlight %}

That was easy, because only Harry has made changes.

Now `git log` in `harry` and `shared` look exactly the same, but `git log` in
Isabelle's repository is out of date. She needs to get those changes. For once, the
command makes sense; it's the opposite of "push", so it must be "pull".

{% highlight text %}
cd ../isabelle
git pull
{% endhighlight %}

Isabelle is now up to date. Here's where we hit another important Git concept. What
Isabelle just did is download a *commit* from the `shared` repository. That is, the
whole changeset came along, with its author information, information about what files
were changed, and information identifying the parent of the commit, so Git can apply
it accurately. So Isabelle's working copy is up-to-date, but her `git log` is also
the same now as `shared` and `harry`.

(Note for the pedantic: here is where I am *not* going to talk about the DAG. Nor
am I going to talk about how "pull" is "fetch" plus "merge" and what each of those
does. Nor will I talk about local and remote references, or the difference between
"master" and "origin/master". Save that stuff for when we need it, which is
*very* rarely if we use Git the way most people use it.)

Wrapping Up
-----------

So far this looks a lot like Subversion. Sure, we saved the change to a local repository
first, and we called that a commit, but really the "push" was a lot like a Subversion
commit, and "git pull" was a lot like "svn up".

Of course, this is true! At the end of the day, we're still managing files and
the changes that we make to those files over time. But as we go along, we'll get into
situations where the behavior is a little different from Subversion, so it's important
to recognize that something "happened" when we did `git commit`, not just when we
did `git push` and `git pull`.

Next time we'll get into what happens in the far more likely case that Harry and Isabelle
need to change things at the same time.


