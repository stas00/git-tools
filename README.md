# git Tools

Various helper tools for working with git and github:

- [git cheat-sheet](./git.txt) - various solutions to git usage
- [how to make PR + tool to create PR branch](./how-to-make-pr/)
- [create TOC in `.md` files](./github-markdown-toc/)
- [rebase/merge a local branch with origin master](./git-rebase/)
- [git targets for Makefiles](./make/)

Useful aliases:

Show a diff of all files modified in the current branch against HEAD:
```
alias brdiff="def_branch=\$(git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@'); git diff origin/\$def_branch..."
```

Same, but ignore white-space differences, adding `--ignore-space-at-eol` or `-w`:
```
alias brdiff-nows="def_branch=\$(git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@'); git diff -w origin/\$def_branch..."
```

List all the files that were added or modified in the current branch compared to HEAD:
```
alias brfiles="def_branch=\$(git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@'); git diff --name-only origin/\$def_branch..."
```

Once we have the list, we can now automatically open an editor to load just added and modified files:
```
alias bremacs="def_branch=\$(git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@'); emacs \$(git diff --name-only origin/\$def_branch...) &"
```
