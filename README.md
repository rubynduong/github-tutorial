# Notes on Git (and GitHub)
This summary contains a quick summary of useful git commands.

For a full tutorial, see https://www.atlassian.com/git/tutorials.

## Concepts
- A commit is a snapshot of a point in time or a point of interest along the timeline of a project's history.
- A branch is a movable pointer to a specific commit.

## Commands

### Getting Started

1. Set up a repository on repository host (e.g. GitHub)
2. Go to desired project directory and execute 

    `git clone <repo url>`
3. Once the remote repository is set up, you will need to add a remote repo url to your local `git config` and set an upstream branch for your local branches. You al

    `git remote add <remote_name> <remote_repo_url>`

### General

To view branches:
    
    git branch 
    [--all]: include remote branches
    [-vv]: see upstream branch

To view logs:

    git log --oneline

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

To transfer commits from the local repository to a remote repository:

    git push [<remote> <branch>]

In most cases (like in `main`), arguments are not necessary as the upstream remote is already set.

### Manipulating Branches

Create a branch at a specific commit:

    git branch <branch-name> <commit#>
    
Create and checkout branch:

    git checkout -b <branch-name>

You can combine both of the above:

    git checkout -b <branch-name> <commit#>
    
---

Push the branch to the remote:

    git push -u <remote-name> <branch-name>

This command will set `<remote-name>` as upstream and link the local branch with the remote branch.

---

Deleting a branch locally:
    
    git branch -d <branch-name>
    
Then push to origin to reflect changes on the remote:
  
    git push origin :<branch-name>

### Undoing Changes

To undo a **local commit** from the current branch:

    git reset <commit#>

The branch pointer is moved back in the chain to 'undo' the changes.

---

To undo a **pushed commit** (to the remote) from the current branch:

    git revert <commit#>

A new commit is added at the end of the branch to 'cancel' the changes.

The preferred method of undoing shared history is `git revert`. A revert is safer than a reset because it will not remove any commits from a shared history. A revert will retain the commits you want to undo and create a new commit that inverts the undesired commit. This method is safer for shared remote collaboration because a remote developer can then pull the branch and receive the new revert commit which undoes the undesired commit.

(From https://www.atlassian.com/git/tutorials/undoing-changes)

---

To amend the last commit:

    git commit --amend
    [--no-edit]: Commit without editing the message
    [-m <message>]: Edit message
