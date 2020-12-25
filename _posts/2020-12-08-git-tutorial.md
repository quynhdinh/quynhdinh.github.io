---
layout: post
title: "A catalog of Git"
---
Shamelessly I have been used Git for some time now but I didn't know many of these stuff. This blog post is a written version of [this video](https://www.youtube.com/watch?v=O5uT6p6VWjY&t). Of course you should learn Git through some type of video tutorial not a blog post like this.
### Git init
Inititalize a git repository in a current directory. The initialized folder called .git which includes config()
### Git status
### Git add và staging
### Git commit
### Git commit --amend
### Git push
### Git push --force
### Git log
Show the commit history of the current branch. To show more of these history type **git log --oneline** to show all commit history in oneliners.
### Introducing branching
The main branch is named **master** where all other branch are supposed to support this branch.

### Git branch
To list all the current branch you type **git branch**
To create a branch A at the branch you are at you type **git branch A**
To create a branch at A named B you type **git branch B A**.

### Git checkout
At first I think the name is quite misleading(shouldn't it be named Git checkin?). **git checkout A** means to hop to branch A.

To checkout to a branch A at the time you create that branch A. You type **git checkout -b A**
### Git merge
### Giải thích git merge fast forward
### Git merge no fast forward
### Git branch -d (xóa branch)
### Git rebase
### Git rebase --preserve-merges(Trường hợp sử dụng)
### Git reset
### Git rebase --preverse-merges
### Pull request
### Git squash
### Git cherry-pick