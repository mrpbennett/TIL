# Creating and switching branches

_Git 2.23 came up with the new `git switch` command_

### Creating a branch

`git branch <branch-name>`

Will create a branch from the branch that you're currently on.

### git switch to new branch

Let’s suppose you want to create a new branch which doesn’t exist and also at the same time we want to switch to it. 

`git switch -c new-branch`

This will create a new branch and then switch to it.

### git switch vs checkout
So what has changed? What’s the difference between  switch and checkout commands? Nothing special. git switch has almost copied and separated out the switching functionality from the git checkout command.

more info [here](https://bluecast.tech/blog/git-switch-branch/)