---
layout: post
title: "Git at speed"
---
### Introduction
Here are some aliases. I use them pretty much everyday. Here, I explain in detailed my aliases use at work. You can find all the code [here](https://github.com/quynhdinh/files/blob/main/bash_aliases) and [here](https://github.com/quynhdinh/files/blob/main/gitconfig)

I normally work on multiple machines and often time code at night after work.
### g
Aliasing `g` as `git`. This is default on linux machine.
### current branch name
I use this a lot in the other aliases

`g at` means `git rev-parse --abbrev-ref HEAD`
### always pull rebase auto-stash
`g pr` means `git pull --autostash --rebase`
### git rebase interative
`g reb 3` means git rebase --interative 3
### git push and git push --force
`g p` means `git push`

`g pf` means `git push --force`
### set upstream
Once you checkout a new branch, you have to type that long set upsteam thing. Automate that!
```bash
function set_upstream {
    CURRENT_BRANCH=$(git rev-parse --abbrev-ref HEAD);
    echo "setting up stream branch $CURRENT_BRANCH"
    git push --set-upstream origin $CURRENT_BRANCH
}
alias up=set_upstream
```
### commit all with a random message
You are lazy and don't want to invent a commit message(because you will rebase it anyway(you are commiting on on your own branch, of course!))
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