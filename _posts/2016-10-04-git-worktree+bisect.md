---
title: "Git: worktree + bisect = ♥"
layout: default
---

[Git worktree](https://git-scm.com/docs/git-worktree) - create branch in separate folder  
[Git bisect](https://git-scm.com/docs/git-bisect) - search for first bad commit

## Motivation
Bisecting may take a while and it makes repository "taken" so no work can be done in it until `bisect` finished.

## Create new worktree  
`> git worktree add -b branch-name <path>`  
`> cd <path>`

## Define "good" and "bad" revisions  
`> git checkout <good-revision>`  
`> # run check to make sure it reports **PASS**`

`> git checkout <bad-revision>`  
`> # run check to make sure it reports **FAIL**`

## Bisecte  
`git bisect start`  
`git bisect good <good-revision>`  
`git bisect bad <bad-revision>`  
`git bisect run <check script>`  
`git bisect reset`

## Clean up worktree  
`rm -rf <path>`  
`git worktree prune`    # clean up `.git/worktrees`

## Summary
Using `bisect` alone is the fastest way to find *buggy* commit. Adding `worktree` to this workflow helps with time saving: you can continue your work while `bisect` is looking for bugs for you. No more *I should wait* or N+ cloned repos.
