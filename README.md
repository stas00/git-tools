# git-tools

Various helper tools to working with git and github

# github-make-pr-branch - auto-create a PR branch (includes forking if needed)

Creating a PR could involve many steps and they vary whether you do it for the first time, or subsequent times and when your forked master goes out of sync with the upstream master. This tool does all the work for you. It creates a PR branch for you, taking care of forking the upstream if needed, syncing your master if needed, setting up your branch for pushing, so all that remains is for you to do your work, commit changes and submit PR.

To run it:

```
curl -O https://raw.githubusercontent.com/stas00/git-tools/master/github-make-pr-branch
chmod a+x github-make-pr-branch
./github-make-pr-branch ssh your-github-username repo_username repo_name new-feature
# of if you don't have ssh setup, use https:
./github-make-pr-branch https your-github-username repo_username repo_name new-feature
```

For example to make a PR branch for https://github.com/stas00/git-tools, do:
```
./github-make-pr-branch ssh your-github-username stas00 git-tools new-feature
```

Then all that's left to do is:

```
cd git-tools-new-feature
# do your changes
git commit <modified files>
git push
# go to github and submit the PR
# done
```
