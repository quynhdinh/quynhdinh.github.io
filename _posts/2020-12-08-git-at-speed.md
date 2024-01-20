---
layout: post
title: "[Tech] Git at speed"
---
### Introduction
Here are some aliases that I use pretty much everyday. Here, I explain in detailed my aliases use at work. You can find all the code [here](https://github.com/quynhdinh/files/blob/main/bash_aliases) and [here](https://github.com/quynhdinh/files/blob/main/gitconfig)

### what's up G
First off, aliasing `g` as `git`. This is default on linux machine.
### current branch name
I use this a lot in the other aliases

`g at` means `git rev-parse --abbrev-ref HEAD`
### always pull rebase auto-stash
`g pr` means `git pull --autostash --rebase`
### rebase interative
`g reb 3` means `git rebase --interative 3`
### push and force push
`g p` means `git push`

`g pf` means `git push --force-with-lease`
### set upstream
Once you checkout a new branch, you have to type that long setting upstream thing. Automate that!
```bash
function set_upstream {
    CURRENT_BRANCH=$(git rev-parse --abbrev-ref HEAD);
    echo "setting up stream branch $CURRENT_BRANCH"
    git push --set-upstream origin $CURRENT_BRANCH
}
alias up=set_upstream
```
### commit all with a random message
You are lazy and don't want to invent a commit message(because you will rebase it anyway(you are commiting on your own branch, of course!))
```bash
function commit_misc {
    CURRENT_BRANCH=$(git rev-parse --abbrev-ref HEAD);
    myArray=("a funny" "a lovely" "a cute" "an adorable" "a pretty" "an elegant" "a charming" "a gorgeous" "a stunning")
    num=${#myArray[@]}
    index=$((1 + $RANDOM % $num))
    adj=${myArray[$index]}
    echo "writing $adj commit and push to branch: $CURRENT_BRANCH"
    git add .
    git commit -m "$adj commit at $(date +%F_%H-%M-%S)"
}
alias misc=commit_misc
```
You might be lazy typing `g p` and want to commit and push in one enter!
```bash
function commit_misc_then_push {
    commit_misc
    git push
}
alias miscp=commit_misc_then_push
```
