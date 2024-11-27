# git best practice 

## what is git 

- Versioning system
- philosophy how to work
    - asynchronous work
    - split work into small packages / tickets

- commits
- branches 
- tags

- local: clone of the repo on your machine (entire history, all branches, tags, ...)
- remote: repo on server, usually called origin.
   - There can be multiple remotes 

### branches 

- 1 main branch, usually main (deprecated: master)
- multiple short-lived feature branches: one per unit of work
- dirt-cheap

- always work on feature branches!
- once your changes are reviewed, feature branch is merged to main 

### tags

- long lived pointer to commit
- usually used for marking release versions


### commits

- commit = difference relative to parent(s)

- 1 commit = 1 logical unit
- changes that belong together
- "will this commit mess-up my future rebases?" 
    - formatting is a separate commit
    - renames, moving, add/delete src-files
    - Note: Windows does not differentiate between `Main.qml` and `main.qml`, git does!

- each commit should compile

- working on larger features: 
    - commit each subfeature separately -> squash later

- commit message:
    - descriptive: Bad: "Changed Param to 42", Good: "Solve the P=NP Problem"
    - prefix: fixup, add, refactor, `<subfeatureName>`, `<ticketNumber>`
    - Team-specific convention may apply
    - Format: https://stackoverflow.com/questions/2290016/git-commit-messages-50-72-formatting

```
Summary (50)

Full description (72 per line)
```


- commit regularly
    - make sure you can remember the changes done
        - leaving the topic -> commit your changes
        - avoid end of day (worst case: week) commit orgy
    - at the end of the day your branch should not contain any uncommitted and un-pushed changes

```
git add filename ...
git commit -m msg
git push
```

.. Example:
    - Neue Datei erstellen
    - und comitten
    - push

## work flow

### reverting

- revert already public (pushed) changes

- usage: when you have to avoid `push -f`

```
git revert <commit>
```


### amending 

- Add changes to the latest local commit 

```
git add ...
git commit --amend 
```

### rebasing 

- keep your branch up to date on a daily base
- start in the day: rebase working branch on upstream main:
    - git fetch origin main:main
    - git rebase main 

#### solving conflicts 

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
git cherry-pick <commit> 
```

For MR:
```
git checkout main
git pull
git checkout -b superFeature
git cherry-pick <commit>
git push --set-upstream origin super Feature 
```

Create MR on gitlab/server





## tools

- tortoise
- vscode plugin: gitLens, gitGraph, Git supercharged



## Advanced git functions

- submodule
- reflog
- worktree
- lfs (immer wieder umstritten)


