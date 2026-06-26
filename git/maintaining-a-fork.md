# Maintaining a Fork and Keeping It Up to Date

When you fork a repo to make your own changes, you need a way to pull in
upstream updates without losing your work. The trick is a second remote
(`upstream`) pointing at the original repo, combined with `rebase` to keep
your commits on top of theirs.

## One-time setup

Add the original repo as a second remote:

```bash
git remote add upstream https://github.com/original-owner/original-repo.git
```

Verify you now have both remotes:

```bash
git remote -v
# origin    https://github.com/you/your-fork.git (fetch)
# origin    https://github.com/you/your-fork.git (push)
# upstream  https://github.com/original-owner/original-repo.git (fetch)
# upstream  https://github.com/original-owner/original-repo.git (push)
```

## Syncing with upstream

Fetch new commits from the original repo and rebase your branch on top:

```bash
git fetch upstream
git rebase upstream/main
```

Using `rebase` instead of `merge` keeps your custom commits on top of the
upstream history without creating noisy merge commits.

## Pushing after a rebase

Because rebase rewrites history, a regular push will be rejected. Use
`--force-with-lease` — it's safer than `--force` because it refuses to push
if someone else has pushed to your branch since you last fetched:

```bash
git push --force-with-lease origin main
```

## The full workflow

```
your changes → commit → push to origin/main
                            ↓
              periodically: git fetch upstream
                            git rebase upstream/main
                            git push --force-with-lease origin main
```

Your fork's `main` is always the source of truth for your custom build.
Upstream is just a source you pull from — your commits always end up on top.
