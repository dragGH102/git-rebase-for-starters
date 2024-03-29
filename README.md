Thanks to https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase for the great overview!

> "Rebase" means: use <branch> as the base for the currently selected branch

This repository:

- starts from `main`
- **forks** into `branch-1`
- `main` is **rebased onto** `branch-1`
- `branch-1` is merged back into master (-no-ff)

Result: **all commits from c1 to c10+** appear in a straight line ( **git log** ) with a single merge commit in `main`

---

Commits list snapshot: ( `git log --pretty=oneline --abbrev-commit` )

__c12+ here__
- d62940d (HEAD -> main, origin/main) 11: fix typo
- 8a6906f (refs/bisect/skip-8a6906fa1ab814bf683e5fd9bbc26c350826af2c) c10: add README
- cf6acf5 Merge branch 'branch-1'
- d83bd34 (branch-1) c9 (branch-1): modify add-3
- 78cf4db c8 (branch-1): add and modify add-4
- b49aed3 c7: rename add-3 to mofify-3
- a09390f c6: add add-4
- 13b0517 c5: rename add-1 to modify-1
- d5120b9 c4: add add-3
- 39ddc5c c3 (branch-1): modify-add-1-without-rename
- cf29294 c2(branch-1): add add-2
- faa2f63 c1: add add-1

## PRO TIPS

### Git rebase interactive and squashing

git rebase `interactive` seems a good approach to cherry-pick multiple commits, e.g. when a subset of commits 
from a> previous merge caused issues. If we don't need the commit information we can **Squash** them!

> Git `bisect` is an interesting tool too for bug hunting! (basic example: https://www.metaltoad.com/blog/beginners-guide-git-bisect-process-elimination )

### Partial changes and rebasing

Attention: if after forking from `base` to `branch-1` we merge partial changes into `base` and then keep working on `branch-1` , t**he rebase is likely to bring back the old version of several files** when rebasing `base` into `branch-1` again!
  
  FROM https://git-scm.com/book/en/v2/Git-Branching-Rebasing
  
  > When you rebase stuff, you’re abandoning existing commits and creating new ones that are similar but different. If you push commits somewhere and others pull them down and base work on them, and then you rewrite those commits with git rebase and push them up again, your collaborators will have to re-merge their work and things will get messy when you try to pull their work back into yours.
REF: 

Rebase is one of the merge strategies git use (along with fast-forward/no-ff): we can rebase also when pulling!

- `git pull` **--rebase** : very useful to avoid conflicts and have a smooth rebase of the pulled commits when the local and remote branches diverge following a rebase of the base branch
