---
layout: chapter
title: "Chapter 1"
description: "Introduction"
next: "02"
tags: []
---

Why This Book
-------------

I recently had some very close friends talk about their hesitation in adopting Git as
opposed to continuing to work with Subversion. I've used Subversion for many
years, and advocated for its use. I have since jumped wholeheartedly on the
Git bandwagon, so I wanted to find a way to tell the story of why I made the
switch and why I think so much of the open source community is now based around
Git and Git-friendly sites like GitHub.

There's so much content out there about Git, and much of it is written at a level that's
way higher than my expertise. But in a way, that's an issue. When you're first starting
out learning something, the questions that you have are way different from the questions
an experienced person has. Once you've won that knowledge, it's almost impossible to go
back and think about what it was like when you were first learning. That puts you in a
bad position to explain to someone else who's brand new.

Git seems particularly prone to this because it's based on some pretty complex notions
of how to think about version control. In particular, once you internalize the concept 
of the Directed Acyclic Graph (DAG) that underlies basically everything in Git, you tend
to want to explain that to new people because (a) it can help you think about how Git works;
and (b) it's cool. Unfortunately, teaching Git from a DAG perspective is IMHO the *worst*
way to teach it to new users because it suggests to them that they have to thoroughly
understand complex concepts from graph theory to use Git effectively. There's also no
question that the Git help pages use Git-specific jargon, which really interferes with
non-experts understanding what a command does.

Style and Motivation
--------------------

I'm hoping in this book to adopt a style that will be accessible to new users.
I'm writing in an informal style, with plenty of first- and second-person
references.  This is not a "dummies" book; I'm not going to talk down to you,
and I'm not going to suggest that you shouldn't learn complex concepts about
Git. But I'm going to try to talk about how I use it and how I see it being
used effectively.

When I first started learning Subversion, there was [a book][svnbook] I found incredibly
helpful, because it focused on *why* Subversion chose the copy-modify-merge model instead
of the checkout-modify-checkin model. It did this by walking through the tool's features
in a way that followed real usage. I hope to present in a similar way.

[svnbook]:http://svnbook.red-bean.com/

I'm calling this book "Conversational Git" both because I'm aiming for a conversational
style and because, when learning a new language, a key goal is to be "conversational" --
able to make basic small talk, even if not quite a native speaker.

Because I wrote this book in a conversational style, it's verbose (like me!) and breezy.
So I hope it's a quick read. I do include a bunch of Git commands in here. If you choose
to follow along running those commands, you'll need to be consistent because some
later things are based on some earlier things. However, when I read tutorials or books
like this, I hate having to follow along typing commands, so I tried to write
in such a way that you can just "get" what the commands are doing from the
context. So don't feel like you have to follow along to follow along, if you
get my meaning.

For the same reason, the ratio of Git commands to text is somewhat lower than
a typical tutorial. I'd rather spend a paragraph explaining or providing
motivation for a one-line command than present all the possible switches
for all the Git commands. That's a [different book][progit], and Scott Chacon 
has done a much better job writing that one than I could anyway.

[progit]:http://git-scm.com/book

Why Not
-------

I'm not writing this book to argue against Subversion in favor of Git. Like I
said, I've used Subversion heavily for many years, and I still advocate for it
when people are looking for version control tools. Where I compare Subversion and
Git, it's an attempt to discuss tradeoffs from the perspective of someone who
likes both tools but tends to use Git by preference.

I'm also not writing this book to advocate for Git versus Mercurial or Bazaar.
I'm not qualified to write that book.

Finally, I am not writing this book to refute people's complaints about Git. In
fact, one of the reasons I wanted to write it is because of Steve Bennett's [10
things I hate about Git][bennett], because I agree with him! Using Git is not
pain-free; I just happen to think it's worth it.

[bennett]:http://steveko.wordpress.com/2012/02/24/10-things-i-hate-about-git/

Dogfooded
---------

We'll start the next chapter momentarily, but first, I want to point out that this
book is [dogfooded][]. It's part of my blog Variegated, which is hosted as a Git
repository on GitHub. So you can fork that repository and get your own copy of
this book to modify. If you make changes, you can send me a pull request so I
can merge your changes into my version. That whole workflow is an essential part
of why Git has become so popular for open-source projects, and a key purpose of this
book is explaining that workflow and why it's so powerful.

The book is written using Markdown and processed using Jekyll. Much love to both
those technologies and to GitHub Pages.

[dogfooded]:http://en.wikipedia.org/wiki/Eating_your_own_dog_food

