+++
author = "Chris Wolfe"
date = "2016-04-01T17:08:39-05:00"
description = "For pull requests, use squash merges"
tags = ["git", "collaboration"]
title = "just squash"

+++

My opinions on whether to squash merge or use merge commits haven’t been
consistent over the past few years. Early on, I was mostly in favor of squashing
commits, then I decided that in order to use certain tools like git bisect, the
complete commit history for a given branch should remain intact.

But, my experience over the past year or show has shown me that preserving this
history isn’t very useful, or at least hasn’t been very useful for me.

While it can be useful to see how a branch has progressed over time; it has been
easier for me to rely on the discussion in the pull request to determine why
certain decisions might have been made. By making a strong effort to keep pull
requests small and narrowly focused, it doesn’t seem as necessary to preserve
the development of the pull request inside of version control - that is, the
pull request hopefully provides enough context for me to understand what that
pull request is doing without me needing to investigate the commits of which the
pull request is comprised.

The main advantage that I see from using a squash strategy, is that the commit
history for a project can be traversed and understood more quickly - each commit
on the master branch should have have tests that pass and code that functions
correctly (correctly in the sense of as designed at the time).

In my opinion, squashing just presents a cleaner and more navigable history to
developers than using merge commits. I find that more beneficial than being able
to understand why a developer decided to commit work at a given point.
