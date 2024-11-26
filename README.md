# git best practice 

## what is git 

- Versioning system
- philosophy how to work
    - asynchronous work
    - splitting in handle working packages 
- commits
- branches 

- local: copy on your machine
- remote: state on server, usually called origin. 
   - There can be multiple remotes 

## branches 

- 1 main branch, usually main or master
- multiple feature branches 

- always work on feature branches!
- once your changes are reviewed, feature branch is merged to main 

## how to commit 

- 1 commit = 1 logical unit
- changes that belong togehter
- can it rebased properly?
- working on larger features: 
    - commit each file separately-> squash later
- commit message:
    - descriptive
    - prefix: fixup, add, refactor 

- commit regulary
    - make sure you can remember the changes done
    - at the end of the day your branch should not contain any uncommited changes
    - push to origin. Local is not enough


git add filename ...
git commit -m msg
git push 

### amending 

Add changes to the latest local commit 

git add ...
git commit --amend 

## rebasing 

- keep your branch up to date on a daily base
- start in the day: rebase working branch on upstream main:
    - git fetch origin main:main
    - git rebase main 

### solving conflicts 

- go over each commit an solve it
    - keep your changes
    - or their changes
    - or both
- if there are many conflicts your commit history might be optimizable 

### interactive rebase 

- before creating a merge request, clean up your branch
- first reorder your commits and
- squash commits that belong togehter 

git rebase -i HEAD~20 

- if there are conflicts -> rethink your commit history 

## tricks 

Useful git commands 

### stash / pop 

- Work is in progress but a collegue requests a immidiate  review of MR in same repo
- stash your uncommited work to be able to change the branch 

```
git stash
git checkout colleguesbranch
# do your review
git checkout mybranch
git stash pop 
```
Note: you can pop changes where ever you want, also on another branch


### reset 

- you are unhappy with a commit
- might want to split it in multiple commits 

```
git rebase -i HEAD~20
# Mark the corresponding commit to stop here
git reset HEAD~
# commit as you now know it better
git rebase --continue
```

### cherry-pick 

- Some commit(s) might be useful in your current branch, but they are not merged yet?
- A commit turns out to be worth a stand alone MR?
- cherry-pick them 

```
Git cherry-pick sha 
```

For MR:
```
git checkout main
git pull
git checkout -b superFeature
git cherry-pick sha
git push --set-upstream origin super Feature 
```

Create MR on gitlab/server