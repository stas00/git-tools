# github-markdown-toc (modified)


This is a fork of the original script: https://github.com/ekalinin/github-markdown-toc

The purpose of this script is to automatically insert or update a Table of Contents (TOC) for `.md` files that contain this pair of tags:
```
<!--ts-->
<!--te-->
```

This script is best executed from inside the folder with `.md` files to avoid `filename.md#` prefix. e.g.:

```
gh-md-toc README.md
```

This repo also includes a handy wrapper script [`gh-md-toc-update-all`](gh-md-toc-update-all) that will recursively search all `.md` files and run this script on.

```
cd dir_with_md_files
gh-md-toc-update-all
```
note it expects to find `gh-md-toc` in your `$PATH`, so either copy it into one of your dirs that are in `$PATH` or edit the script to hardcode its location.

# My modifications:

- insert TOC header tag w/ TOC's contents
- in --insert mode don't dump the TOC to STDOUT (need to be quiet)
- delete tmp and orig files (need not leave garbage behind)
- removed banner (need to be quiet)
