# git-rebase

This is a little script that:
1. syncs the upstream master with origin master
2. and then rebases the current branch to upstream master using the merge strategy

```
git fetch upstream
git checkout master
git merge --no-edit upstream/master
git push --set-upstream origin master

git checkout -
git pull --rebase
git merge origin/master
```

If there are any conflicts after running it, resolve those, run `git commit`.

And finally run `git push`.

The script requires an initial clean state, so if there are any uncommitted
changes you can either `git stash` them or `git commit` them. If you used `git
stash`, you restore those changes back with `git stash pop`. So your process may
look like:

```
git commit
git-rebase
```

or if you aren't ready to commit:

```
git stash
git-rebase
git stash pop
```

Note:

I recently changed this script to automatically handle forked and non-forked branches. So I can just invoke `git-rebase` on any branch and not worry. 

The issue is how to detect that correctly. Since I typically use [github-make-pr-branch](https://github.com/stas00/git-tools/blob/master/how-to-make-pr/github-make-pr-branch) for making branches, which nicely sets up a variety of things to make PRs easily, the script sets upstream for `git remote` - so if `git remote get-url upstream` is set in such branches, whereas in a normal non-forked branch there is no upstream. If you know of a more universal way to programmatically detect when we need to first merge upstream/master into origin/master and when not please let me know.

