+++
author = "Chris Wolfe"
date = "2016-04-01T17:08:39-05:00"
description = "For PRs, just use squashed commits"
tags = ["git", "collaboration"]
title = "just squash"

+++

My opinions on whether to squash or use merge commits haven't been
consistent over the past few years. Early on, I was mostly
in favor of squashing commits, then I decided that in order to use
certain tools like `git bisect`, that the complete commit history for a
given branch should remain intact.

But, my experience over the past year or show has shown me that preserving
this history isn't very useful, or at least hasn't been very useful for me.

While it can be useful to see how a branch has progressed over time; it
has been easier for me to rely on the discussion present on the pull request to
determine why certain decisions might have been made while the branch was being
developed. Also, by making a strong effort to keep branches and pull requests
small and narrow in focus, it doesn't seem as necessary to preserve the
development of the pull request inside the VCS itself.

The main advantage that I see from using a squashed history, is that the commit
history for a project can be traversed and understood more quickly - each commit
on the master branch, should have tests that pass and code that
functions as tested.

Overall, it seems like squashing just presents a cleaner and more navigable
history to developers and that feels like a more compelling reason than being
able to view the branch was developed.
