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

