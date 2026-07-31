# Rebase vs Squash

Both `git rebase` and squashing rewrite history, but they solve different
problems and are often confused because squashing is usually *done with*
rebase.

## Rebase

`git rebase <branch>` takes the commits on your current branch and replays
them one by one on top of `<branch>`, giving you a new, linear sequence of
commits instead of a merge commit. It changes *where* your commits sit in
history, not how many of them there are — three commits in, three commits
out (just with new hashes and a new base).

```bash
git checkout feature
git rebase main
```

This is useful when `main` has moved on since you branched, and you want
your feature branch to look like it was built on the latest `main`, without
a merge commit cluttering the log.

## Squash

Squashing takes multiple commits and combines them into one. It changes
*how many* commits there are, not necessarily where they sit. You can squash
without rebasing at all, e.g. with `git reset --soft`:

```bash
git reset --soft HEAD~3
git commit -m "One clean commit for three messy ones"
```

This resets the branch pointer back 3 commits but keeps all the changes
staged, so a single commit replaces the three.

## Interactive rebase does both at once

In practice, most people squash *via* an interactive rebase, which is why
the two get conflated:

```bash
git rebase -i main
```

In the editor, changing `pick` to `squash` (or `s`) on a commit folds it
into the one above it. So `rebase -i` can replay commits onto a new base
**and** squash them into fewer commits, in the same operation.

## The distinction that matters

- **Rebase** = replay commits on a different base (linearize history).
- **Squash** = combine commits into fewer commits (simplify history).

A PR merge strategy of "squash and merge" on GitHub is really just: take all
commits on the branch, squash them into one, then fast-forward `main` to
include it — no interactive rebase required on your end, GitHub does it for
you.

Docs: https://git-scm.com/docs/git-rebase
