# Notes on Git (and GitHub)
This summary contains a quick summary of useful git commands.

For a full tutorial, see https://www.atlassian.com/git/tutorials.

## Concepts
- A commit is a snapshot of a point in time or a point of interest along the timeline of a project's history.
- A branch is a movable pointer to a specific commit.

## Commands
### General

To view branches:
    
    git branch

To view logs:

    git log --online

To view current status:

    git status

### Synchronising

To download updates without affecting local repository:
    
    git fetch
    
To download updates and immediately create a merge commit for the new remote content:

    git pull 

Equivalent to `git fetch` + `git merge`.

To download updates and immediately rebase:

    git pull --rebase <remote>

Equivalent to `git fetch` + `git rebase`.

### Making Changes

`git push`

### Manipulating Branches

Create a branch at a specific commit:

    git branch <branch-name> <commit#>
    
Create and checkout branch:

    git checkout -b <branch-name>

You can combine both of the above:

    git checkout -b <branch-name> <commit#>
    
---

Deleting a branch:
    
    git branch -d <branch-name>
    
Push to origin to reflect changes on the remote:
  
    git push origin :<branch-name>

### Undoing Changes

To undo a **local commit** from the current branch, do

    git reset <commit#>

The branch pointer is moved back in the chain to 'undo' the changes.

---

To undo a **pushed commit** (to the remote) from the current branch, do

    git revert <commit#>

A new commit is added at the end of the branch to 'cancel' the changes.

The preferred method of undoing shared history is `git revert`. A revert is safer than a reset because it will not remove any commits from a shared history. A revert will retain the commits you want to undo and create a new commit that inverts the undesired commit. This method is safer for shared remote collaboration because a remote developer can then pull the branch and receive the new revert commit which undoes the undesired commit. (From https://www.atlassian.com/git/tutorials/undoing-changes)

---

Amend the last commit
- To add another file:

      git commit --amend --no-edit
      
- To edit commit message:

      git commit --amend -m <message>
