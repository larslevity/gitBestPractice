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
- each commit should compile

## work flow

### staging

- collect changes for commit

```
git add <fileName> [...]    # stage specified file
git add -i                  # stage only parts of a changed file
git add .                   # stage all git-known files
```

### committing

- "will this commit mess-up my future rebases?" 
    - formatting is a separate commit
    - renames, moving, add/delete src-files
    - Note: Windows does not differentiate between `Main.qml` and `main.qml`, git does!


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
git commit -m msg
git push
```

#### Example

    - `featureBranch` erstellen
    - Neue Datei erstellen `main.cpp`
    - commit
    - push
    - main function commit -> suitable msg
    - push


### reverting

- revert already public (pushed) changes
- usage: when you have to avoid `push -f`

```
git revert <commit>
git push
```

#### Example
    - Letzten commit reverten
    - Revert reverten



### amending 

- Add changes to the latest **local** commit 
- And/Or change commit message of latest **local** commit

```
git add ...
git commit --amend 
git push
```

#### Example
    - Hello World -> Hallo Welt
    - amend to last commit


### handling parallel work

```
git status  # clean
git checkout main
git checkout -b superUrgentFeatureRequest
# implement
git add .
git commit -m "add: superUrgentFeature"
git push --set-upstream origin superUrgentFeatureRequest
```

#### Example
    - Neue Branch
    - Add Readme
    - commit
    - push
    - merge


### merging

- create MR on server
- Find somebody to review
- once review is finished, merge!


### rebasing 

- keep your branch up to date on a daily base
- start in the day: rebase working branch on upstream main


```
git fetch origin main:main  # start of the day
git checkout featureBranch
git rebase main
```

#### Example
    - back to featureBranch
    - rebase on updated main
    - merge


#### solving conflicts 

- go over each commit an solve it
    - keep your changes
    - or their changes
    - or both
- if there are many conflicts your commit history might be optimizable 

#### Example
    - Dennis macht Türkisch
    - Lars macht Englisch
    - Dennis merged
    - Lars rebased -> kaputt




---
break

---



### stash / pop 

- Work is in progress but a colleague requests a immediate  review of MR in same repo
- stash your uncommitted work to be able to change the branch 

```
git stash
git checkout colleguesbranch
# do your review
git checkout mybranch
git stash pop 
```

Note: you can pop changes where ever you want, also on another branch


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
git push --set-upstream origin superFeature 
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


### interactive rebase 

- before creating a merge request, clean up your branch
- first reorder your commits and
- squash commits that belong together 

```
git rebase -i HEAD~<numCommits>
# tidy up history
git push -f  #--force-with-lease  # use careful
```

- if there are conflicts -> rethink your commit history 


### reset 

- you are unhappy with a commit
- might want to split it in multiple commits 

```
git rebase -i HEAD~<numCommits>
# Mark the corresponding commit to stop here
git reset HEAD~
# commit as you now know it better
git rebase --continue
```
