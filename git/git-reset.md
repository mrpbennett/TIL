# Git Reset

I have have often commited some files and it's broke something that I couldn't always figure out how to fix. I often wanted to just get back to the commit which I knew worked.

This is where `git reset` comes in. There are many options for reset.

`git reset [<mode>] [<commit>]`
This form resets the current branch head to `<commit>` and possibly updates the index (resetting it to the tree of `<commit>`) and the working tree depending on `<mode>`. If `<mode>` is omitted, defaults to --mixed. The `<mode>` must be one of the following:

`--soft`
Does not touch the index file or the working tree at all (but resets the head to `<commit>`, just like all modes do). This leaves all your changed files "Changes to be committed", as git status would put it.

`--mixed`
Resets the index but not the working tree (i.e., the changed files are preserved but not marked for commit) and reports what has not been updated. This is the default action.

If `-N` is specified, removed paths are marked as intent-to-add.

`--hard`
Resets the index and working tree. Any changes to tracked files in the working tree since `<commit>` are discarded.

`--merge`
Resets the index and updates the files in the working tree that are different between `<commit>` and HEAD, but keeps those which are different between the index and working tree (i.e. which have changes which have not been added). If a file that is different between `<commit>` and the index has unstaged changes, reset is aborted.

In other words, `--merge` does something like a `git read-tree -u -m` `<commit>`, but carries forward unmerged index entries.

--keep
Resets index entries and updates files in the working tree that are different between `<commit>` and HEAD. If a file that is different between `<commit>` and HEAD has local changes, reset is aborted.

`--[no-]recurse-submodules`
When the working tree is updated, using `--recurse-submodules` will also recursively reset the working tree of all active submodules according to the commit recorded in the superproject, also setting the submodules' HEAD to be detached at that commit.