# github PR made easy

<!--ts-->

Table of Contents
-----------------

   * [github PR made easy](#github-pr-made-easy)
      * [Table of Contents](#table-of-contents)
      * [github-make-pr-branch tool](#github-make-pr-branch-tool)
      * [How to Make a Pull Request (PR)](#how-to-make-a-pull-request-pr)
         * [Helper Program](#helper-program)
         * [Step 1. Start With a Synced Fork Checkout](#step-1-start-with-a-synced-fork-checkout)
            * [1a. First time](#1a-first-time)
            * [1b. Subsequent times](#1b-subsequent-times)
         * [Step 2. Create a Branch](#step-2-create-a-branch)
         * [Step 3. Write Your Code](#step-3-write-your-code)
         * [Step 4. Push Your Changes](#step-4-push-your-changes)
         * [Step 5. Submit Your PR](#step-5-submit-your-pr)
         * [How to Keep Your Feature Branch Up-to-date](#how-to-keep-your-feature-branch-up-to-date)
         * [How To Reset Your Forked Master Branch](#how-to-reset-your-forked-master-branch)
         * [Where am I?](#where-am-i)
<!--te-->

## github-make-pr-branch tool

Creating a github Pull Request (PR) could involve many steps and they vary whether you do it for the first time, or subsequent times and when your forked master goes out of sync with the upstream master. This tool does all the work for you. It creates a PR branch for you, taking care of forking the upstream if needed, syncing your master if needed, setting up your branch for pushing, so all that remains is for you to do your work, commit changes and submit a PR.

If you'd like to understand what it does, there is a step-by-step guide covering all the details in the next section of this document.

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




## How to Make a Pull Request (PR)

This is a github specific guide, but most of the information here should be applicable to other git hosting services.

For the sake of the examples in this guide I will assume that you want to submit a PR to this very repo: https://github.com/stas00/git-tools/. So once you understood the process you can apply it to any github repo.

There are two urls that you need to understand.

1. The original repo url is comparised of the repo's user and repo's name:

   ```
   https://github.com/stas00/git-tools/
                        |       |
                    repo_user repo_name
   ```
2. Once you fork the original repo, and say your username is `USERNAME`, you end up with a new url that looks like:
   ```
   https://github.com/USERNAME/git-tools/
                        |       |
                    repo_user repo_name
   ```
From now on you will be working with that 2nd url to do all your work, and every so often when you're ready to contribute something to the upstream original repository, you'd send a Pull Request (PR) there, offering your contributions, which then will be either accepted and merged into the main code base, or rejected and then you will still have a copy of your contributions.


### Helper Program

As you will discover shortly there are several steps to making a PR, some of them are quite technical, but luckily this repo includes a smart bash program [`github-make-pr-branch`](#github-make-pr-branch-tool) that can do all the heavy lifting of the first 2 steps for you. Then you just need to do your work, commit changes and submit your PR.


### Step 1. Start With a Synced Fork Checkout

#### 1a. First time

If you made the fork of the desired repository already, proceed to section 1b.

If it's your first time, you just need to make a fork of the original repository:

1. Go to [https://github.com/stas00/git-tools](https://github.com/stas00/git-tools) and in the right upper corner click on `[Fork]`. This will generate a fork of this repository, and you will be redirected to
 github.com/USERNAME/git-tools.

2. Clone the main repository fork. Click on `[Clone or download]` button to get the clone url and then clone your repository.

   * Choose the SSH option if you have SSH configured with github and run:

   ```
   git clone git@github.com:USERNAME/git-tools.git git-tools-fork
   ```
   * otherwise choose the HTTPS option:

   ```
   git clone https://github.com/USERNAME/git-tools.git git-tools-fork
   ```

   Make sure the url has your username in it. If the username is `git-tools` you're cloning the original repo and not your fork. This will not do what you need.

   Then move into the newly created directory:

   ```
   cd git-tools-fork
   ```

   And let's setup this fork to track the upstream:

   * Using SSH:

   ```
   git remote add upstream git@github.com:stas00/git-tools.git
   ```
   * Using HTTPS:

   ```
   git remote add upstream https://github.com/stas00/git-tools.git
   ```

   You can check your setup:
   ```
   git remote -v
   ```

   It should show:

   * If you used SSH:

   ```
   origin  git@github.com:USERNAME/git-tools.git (fetch)
   origin  git@github.com:USERNAME/git-tools.git (push)
   upstream  git@github.com:stas00/git-tools.git (fetch)
   upstream  git@github.com:stas00/git-tools.git (push)
   ```
   * If you used HTTPS:

   ```
   origin  https://github.com/USERNAME/git-tools.git (fetch)
   origin  https://github.com/USERNAME/git-tools.git (push)
   upstream  https://github.com/stas00/git-tools.git (fetch)
   upstream  https://github.com/stas00/git-tools.git (push)
   ```

   You can now proceed to step 2.

#### 1b. Subsequent times

If you make a PR right after you made a fork of the original repository, the two repositories are aligned and you can easily create a PR. If time passes the original repository starts diverging from your fork, so when you work on your PRs you need to keep your master fork in sync with the original repository.

You can tell the state of your fork, by going to https://github.com/USERNAME/git-tools and seeing something like:

```
This branch is 3 commits behind git-tools:master.
```

So, let's synchronize the two:

1. Place yourself in the `master` branch of the forked repository:

   * Either you go back to a repository you checked out earlier and switch to the `master` branch:

   ```
   cd git-tools-fork
   git checkout master
   ```

   * or you make a new clone

   ```
   git clone git://github.com/USERNAME/git-tools.git git-tools-fork
   cd git-tools-fork
   git remote add upstream git@github.com:stas00/git-tools.git
   ```

   and set things up as before (except for the `fastprogress` repository):
   ```
   tools/run-after-git-clone
   ```

   Use the https version https://github.com/USERNAME/git-tools if you don't have ssh configured with github.

2. Sync the forked repository with the original repository:

   ```
   git fetch upstream
   git checkout master
   git merge --no-edit upstream/master
   git push
   ```

   Now you can branch off this synced `master` branch.

   Validate that your fork is in sync with the original repository by going to https://github.com/USERNAME/git-tools and checking that it says:

   ```
   This branch is even with git-tools:master.
   ```
   Now you can work on a new PR.


### Step 2. Create a Branch

It's very important that you **always work inside a branch**. If you make any commits into the `master` branch, you will not be able to make more than one PR at the same time, and you will not be able to synchronize your forked `master` branch with the original without doing a reset. If you made a mistake and committed to the `master` branch, it's not the end of the world, it's just that you made your life more complicated. This guide will explain how to deal with this situation.


1. Create a branch with any name you want, for example `new-feature-branch`, and switch to it. Then set this branch's upstream, so that you could do `git push` and other git commands without needing to pass any more arguments.

   ```
   git checkout -b new-feature-branch
   git push --set-upstream origin new-feature-branch
   ```

### Step 3. Write Your Code

This is where the magic happens.

Create new code, fix bugs, add/correct documentation.

If there is a way to test the codebase, do it now, so that you won't discover that once you submitted your PR, the Continuous Integration (CI) server reports a failure and your PR won't get merged. The testing is often done with `make test`, but can be different or not have a way to do the testing at all.

### Step 4. Push Your Changes

1. When you're happy with the results, commit the new code:

   ```
   git commit -a
   ```

   `-a` will automatically commit changes to any of the repository files.

   If you created new files, first tell git to track them:

   ```
   git add newfile1 newdir2 ...
   ```
   and then commit.

2. Finally, push the changes into the branch of your fork:

   ```
   git push
   ```

### Step 5. Submit Your PR

1. Go to github and make a new Pull Request:

   Usually, if you go to https://github.com/USERNAME/git-tools github will notice that you committed to a new branch and will offer you to make a PR, so you don't need to figure out how to do it.

   If for any reason it's not working, go to https://github.com/USERNAME/git-tools/tree/new-feature-branch (replace `new-feature-branch` with the real branch name, and click `[Pull Request]` in the right upper corner.

If you work on several unrelated PRs, make different directories for each one, ideally using the same directory name as the branch name, to simplify things.



### How to Keep Your Feature Branch Up-to-date

Normally you don't need to worry about updating your feature branch to synchronize with the `git-tools` code base (upstream master). The only time you must perform the update is when the same code you have been working on has undergone changes in the `master` branch. So when you submit a PR, github will tell you that there is a merge conflict.

You could update your feature branch directly, but it's best to update the master branch of your fork, first.

* Step 1: sync your forked `master` branch:

   ```
   cd my-cool-feature # your git-tools fork clone directory
   git fetch upstream
   git checkout master
   git merge --no-edit upstream/master
   git push --set-upstream origin master
   ```

* Step 2: update your feature branch `my-cool-feature`:

   ```
   git checkout my-cool-feature
   git merge origin/master
   ```

* Step 3:  resolve any conflicts resulting from the merge (using your editor or a special merge tool), followed by `git add` to the files which had conflict.

* Step 4: push to github the updates to your branch:

   ```
   git push
   ```

   If your PR is already open, github will automatically update its status showing the new commits and the conflict shouldn't be there any more if you followed the steps above.



### How To Reset Your Forked Master Branch

If you haven't been careful to create a branch, and committed to the `master` branch of your forked repository, you no longer will be able to sync it with the original repository, without resetting it. And when you will want to create a branch, it'll have issues during PR, since it will be made against a diverged origin.

Of course, the brute-force approach is to go to github, delete your fork (which will delete any of the work you have done on this fork, including any branches, so be very careful if you decided to do that, since there will be no way to recover your data).

A much safer approach is to reset the `HEAD` of your forked `master` with the `HEAD` of the original repository:

If you haven't setup up the upstream, do it now:
   ```
   git remote add upstream git@github.com:stas00/git-tools.git
   ```

and then do the reset:
   ```
   git fetch upstream
   git update-ref refs/heads/master refs/remotes/upstream/master
   git checkout master
   git stash
   git reset --hard upstream/master
   git push origin master --force
   ```

### Where am I?

Now that you have the original repository, the forked repository and its branches how do you know which of the repository and the branch you are currently in?

* Which repository am I in?

   ```
   git config --get remote.origin.url | sed 's|^.*//||; s/.*@//; s/[^:/]\+[:/]//; s/.git$//'
   ```
   e.g.: `stas00/git-tools`

* Which branch am I on?

   ```
   git branch | sed -n '/\* /s///p'
   ```
   e.g.: `new-feature-branch7`

* Combined:

   ```
   echo $(git config --get remote.origin.url | sed 's|^.*//||; s/.*@//; s/[^:/]\+[:/]//; s/.git$//')/$(git branch | sed -n '/\* /s///p')
   ```
   e.g.: `stas00/git-tools/new-feature-branch7`

But that's not a very efficient process to constantly ask the system to tell you where you are. Why not make it automatic and integrate this into your bash prompt (assuming that use bash).
