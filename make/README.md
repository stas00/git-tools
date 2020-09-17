# Makefile git snippets

Once upon a time I wrote a complex Makefile for fastai. Some of those
git-related bits might be useful to some of you. Hopefully, they are
self-explanatory. The full `Makefile` and a very detailed usage doc can be found  [here](https://github.com/stas00/toolbox/tree/master/make).

```

define get_cur_branch
$(shell git branch | sed -n '/\* /s///p')
endef

define echo_cur_branch
@echo Now on [$(call get_cur_branch)] branch
endef

cur_branch = $(call get_cur_branch)

##@ git helpers

git-pull: ## git pull
	@echo "\n\n*** Making sure we have the latest checkout"
	git checkout master
	git pull
	git status

git-clean-check:
	@echo "\n\n*** Checking that everything is committed"
	@if [ -n "$(shell git status -s)" ]; then\
		echo "git status is not clean. You have uncommitted git files";\
		exit 1;\
	else\
		echo "git status is clean";\
    fi

git-check-remote-origin-url:
	@echo "\n\n*** Checking `git config --get remote.origin.url`"
	@perl -le '$$_=shift; $$u=q[git@github.com:fastai/fastai.git]; $$_ eq $$u ? print "Correct $$_" : die "Expecting $$u, got $$_"' $(shell git config --get remote.origin.url)

sanity-check: git-clean-check git-check-remote-origin-url
	@echo "\n\n*** Checking master branch version: should always be: X.Y.Z.dev0"
	@perl -le '$$_=shift; $$v="initial version: $$_"; /\.dev0$$/ ? print "Good $$v" : die "Bad $$v, expecting .dev0"' $(version)

sanity-check-hotfix: git-clean-check git-check-remote-origin-url
	@echo "\n\n*** Checking branch name: expecting release-X.Y.Z"
	@perl -le '$$_=shift; $$br="current branch: $$_"; /^release-\d+\.\d+\.\d+/ ? print "Good $$br" : die "Bad $$br, expecting release-X.Y.Z"' $(cur_branch)

prev-branch-switch:
	@echo "\n\n*** [$(cur_branch)] Switching to prev branch"
	git checkout -
	$(call echo_cur_branch)

# also do a special sanity check for broken git setups that switch to private fork on branch
release-branch-create:
	@echo "\n\n*** [$(cur_branch)] Creating release-$(version) branch"
	git checkout -b release-$(version)
	$(call echo_cur_branch)
	$(MAKE) git-check-remote-origin-url

release-branch-switch:
	@echo "\n\n*** [$(cur_branch)] Switching to release-$(version) branch"
	git checkout release-$(version)
	$(call echo_cur_branch)

master-branch-switch:
	@echo "\n\n*** [$(cur_branch)] Switching to master branch: version $(version)"
	git checkout master
	$(call echo_cur_branch)

commit-version: ## commit and tag the release
	@echo "\n\n*** [$(cur_branch)] Start release branch: $(version)"
	git commit -m "starting release branch: $(version)" $(version_file)
	$(call echo_cur_branch)

# in case someone managed to push something into master since this process
# started, it's now safe to git pull (which would avoid the merge error and
# break 'make release'), as we are no longer on the release branch and new
# pulled changes won't affect the release branch
commit-dev-cycle-push: ## commit version and CHANGES and push
	@echo "\n\n*** [$(cur_branch)] pull before commit to avoid interactive merges"
	git pull

	@echo "\n\n*** [$(cur_branch)] Start new dev cycle: $(version)"
	git commit -m "new dev cycle: $(version)" $(version_file) CHANGES.md

	@echo "\n\n*** [$(cur_branch)] Push changes"
	git push

commit-release-push: ## commit CHANGES.md, push/set upstream
	@echo "\n\n*** [$(cur_branch)] Commit CHANGES.md"
	git commit -m "version $(version) release" CHANGES.md || echo "no changes to commit"

	@echo "\n\n*** [$(cur_branch)] Push changes"
	git push --set-upstream origin release-$(version)

commit-hotfix-push: ## commit version and CHANGES and push
	@echo "\n\n*** [$(cur_branch)] Complete hotfix: $(version)"
	git commit -m "hotfix: $(version)" $(version_file) CHANGES.md

	@echo "\n\n*** [$(cur_branch)] Push changes"
	git push

tag-version-push: ## tag the release
	@echo "\n\n*** [$(cur_branch)] Tag $(version) version"
	git tag -a $(version) -m "$(version)" && git push origin tag $(version)

# check whether there any commits besides fastai/version.py and CHANGES.md
# from the point of branching of release-$(version) till its HEAD. If
# there are any, then most likely there are things to backport.
backport-check: ## backport to master check
	@echo "\n\n*** [$(cur_branch)] Checking if anything needs to be backported"
	$(eval start_rev := $(shell git rev-parse --short $$(git merge-base master origin/release-$(version))))
	@if [ ! -n "$(start_rev)" ]; then\
		echo "*** failed, check you're on the correct release branch";\
		exit 1;\
	fi
	$(eval log := $(shell git log --oneline $(start_rev)..origin/release-$(version) -- . ":(exclude)fastai/version.py" ":(exclude)CHANGES.md"))
	@if [ -n "$(log)" ]; then\
		echo "!!! These commits may need to be backported:\n\n$(log)\n\nuse 'git show <commit>' to review or go to https://github.com/fastai/fastai1/compare/release-$(version) to do it visually\nFor backporting see: https://docs-dev.fast.ai/release#backporting-release-branch-to-master";\
	else\
		echo "Nothing to backport";\
    fi



```

