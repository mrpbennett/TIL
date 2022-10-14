# Pushing to a new remote branch

To push a new project to a new remote repo hosted on say Github. First you will
need to create a new repo in GH.

Lets say we created `git@github.com:mrpbennett/GitGit.git`

Now we need to push our content to the repo. Make sure you're within the
directory of the project.

```zsh
get remote add origin git@github.com:mrpbennett/GitGit.git
```

The above adds a new remote reference to the repo that was created on GH and
adds an origin.
