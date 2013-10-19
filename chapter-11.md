---
layout: chapter
title: "Chapter 11"
description: "Flow"
previous: "10"
tags: []
---

Workflow with Git
-----------------

I've presented a lot of topics around using Git on a project. I've advocated for
the use of feature branches. What's left is to talk about how to get started
using Git on a typical project, including both tools and workflow.

Tools
-----

I mentioned at the beginning that I wouldn't talk about setting up shared
repositories on a server, because that kind of thing is best handled in a 
GitHub-like tool. (There are many examples: GitHub itself, Gitlab, Gitorious,
Atlassian Stash.) Any of those tools can be used on the Internet or installed
on a local server. They provide user management and repository management. They
allow authorized users to push to and pull from shared repositories, typically
using SSH (ideally using key-based authentication) or HTTP/S.

So they take away a lot of the pain of setting up a server to host Git. But
they also enable a Git workflow with feature branches, in a number of 
important ways:

* They provide a way to list feature branches, show activity, and inspect the
  content of the repository.
* They notice when a new feature branch is pushed to the server, and offer
  to create a "pull request".
* They provide a way to review the code in the feature branch and make comments.
* They notice subsequent updates to feature branches and fold them into existing
  pull requests.
* They allow someone to accept the pull request, automatically folding in the
  feature branch. (This also enforces the principle that feature branches should
  merge cleanly.)
* They integrate with build tools so feature branches can be built and tested
  before their changes are integrated.

Agile tells us to value working software and interactions between people. The
workflow that GitHub-like tools enable is one where small changes can be quickly
and easily built, tested and reviewed before being integrated. I'm indebted to
Scott Chacon for [describing][ghflow] how GitHub does their own development work;
it's a very lucid, detailed explanation that's well worth reading.

[ghflow]:http://scottchacon.com/2011/08/31/github-flow.html

Workflow
--------

So once you've picked a tool and installed it, the other big question is, how
will your team use Git as part of your workflow? Does everything have to be
on a feature branch, or is it OK to commit to `master` for hotfixes? Will your
team have to maintain branches for older versions and backport high priority
fixes? Do you need an intermediate branch for your next "unstable" version,
as described in [Git Flow][gitflow]?

[gitflow]:http://nvie.com/posts/a-successful-git-branching-model/

Each team will identify its own answer to these questions, as well as decisions
like what backlog / ticket system will be used to manage work, and whether it
will be used for all work or only for problem reporting.

Conclusion
----------

There are tradeoffs in deciding to use Git as opposed to any other version control
tool. There may be a learning curve for some members of the team, or a need to
convince leadership of the value of introducing yet another tool. 

Just as in a change from a checkout-modify-checkin to a copy-modify-merge model,
there is a conceptual change. With copy-modify-merge, we had the new idea that
people could change the same file at the same time and wait until commit time
to resolve conflicts. With distributed version control, commit and merge are kept
completely separate, so committing and branching becomes as cheap as possible
and we have total control over when to merge.

Whether Git is worth the tradeoff is a per-team and per-person decision. My hope
is that this book has been an enjoyable read and helped to make the concepts
clearer so the tradeoff decision can be made for the right reasons.

