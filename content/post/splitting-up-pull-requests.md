+++
title = "Splitting up pull requests"
date = "2016-01-23"
+++

I've been working on greenfield projects for the past month. Most of my branches
that have started out with a small, limited scope have tended to balloon as I've
learned more about what the requirements really are. This has meant that I've
needed to break down branches into smaller units.
[Git](https://www.git-scm.com/) has tools that make this a fairly painless process.

## Checking out hunks from a base branch
This is useful when a branch needs splitting up and its commit history doesn't
contain much useful information.

The commands are:

```bash
# checkout a fresh branch for the smaller scope
git checkout -b branch-with-smaller-scope master
# start checking out work from the source branch
git checkout -p the-branch-with-all-of-the-work -- src/foo/thing.clj
```
This command will checkout hunks from `src/foo/thing.clj` just as you
would if you were using `git add -p`. If `git` can't decide how
to split up a hunk, you can manually edit the hunk in your text editor.

## Rebasing
I mainly use interactive rebasing to rewrite and destroy commit history.
My typical use-case is that I've written a large feature branch that contains
work-in-progress and fixup commits. Most of the time, I'd prefer not
to merge these types of throwaway commits.

The command to use is:

```bash
git rebase -i the-base-branch
```

This will open up an interactive prompt that allows you to select commits to alter.
The prompt provides a menu allowing the user to select which commits they would
like to leave unaltered, edit, squash, treat as fixup commits, or destroy.
I tend to use fixup commits when a commit is simply "fixed typo in last commit" and the like.
I also like to squash some commits and edit their commit message.
The interactive rebase interface makes this process painless.

## Cherry-picking
I use cherry-picking when a commit contains work and commit history that I would
like to pull into another branch. To be honest, I use cherry-picking less
frequently simply because my commit history tends to contain a fair amount of
work-in-progress commits. Outside of keeping a better commit history, a solution
to this would be to:

1. rebase the work-in-progress commits into more meaningful commits
2. cherry-pick those commits into the target branch (alternatively, rebasing could achieve the exact same thing)

Cherry-picking is a pretty simple process. It is possible to select a single commit or range of commits

```bash
# selects a single commit and apply it to the current branch
git cherry-pick hash-of-target-commit

# selects the sixth, fifth, fourth, and thrid most recent commits from target-branch
# and apply them to the current branch
git cherry-pick target-branch~5 target-branch~2
```
Running these commands will attempt to apply commits that were cherry-picked to
the current branch. If there are any conflicts, you will need to resolve the
process similarly to how you would during a merge.

For more information, here are the docs on
[checking out patches](https://git-scm.com/docs/git-checkout),
[rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing),
and [cherry-picking](https://git-scm.com/docs/git-cherry-pick)
