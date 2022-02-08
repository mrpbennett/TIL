# Cloning from a certain branch

Now let's clone a specific branch from our demo repository. There are two ways to clone a specific branch. You can either:

- Clone the repository, fetch all branches, and checkout to a specific branch immediately.
- Clone the repository and fetch only a single branch.

```zsh
git clone -b <branchname> <remote-repo-url>
```

Example: 

```zsh
git clone -b pb-test-2 https://github.com/mrpbennett/signal_debugger.git
```

With this, you fetch all the branches in the repository, checkout to the one you specified, and the specific branch becomes the configured local branch for `git push` and `git pull` . But you still fetched all files from each branch.

### 2nd option

```zsh
git clone -b <branchname> --single-branch <remote-repo-url>
```

This performs the same action as option one, except that the `--single-branch` option was introduced in Git version 1.7.10 and later. It allows you to only fetch files from the specified branch without fetching other branches.
