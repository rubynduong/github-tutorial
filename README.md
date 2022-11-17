# Git notes

## Useful resources

Tutorial: https://www.atlassian.com/git/tutorials

Documentation: https://git-scm.com/book/en/v2https://github.com/zacharyt-cs/git-notes

Cheat sheet (PDF): https://training.github.com/downloads/github-git-cheat-sheet.pdf

Cheat sheet (interactive): https://ndpsoftware.com/git-cheatsheet.html#loc=workspace

----
## Concepts
- A **commit** is a snapshot of a point in time or a point of interest along the timeline of a project's history.
- A **branch** is a movable pointer to a specific commit.
- **Index/staging area** is where you add files to be part of the next commit.

----
## Getting Started
| Function                                                                                          | Command                                          | Option |
|---------------------------------------------------------------------------------------------------|--------------------------------------------------|-------:|
| Create repository                                                                                 | `git init`                                       |        |
| Clone repository                                                                                  | `git clone <path>`                               |        |
| Add a remote repo url to your local `git config` + set an upstream branch for your local branches | `git remote add <remote_name> <remote_repo_url>` |        |

----
## General
| Function                                | Command             | Option                                    |
|---------------------------------------- |--------------       |-------------------------------------:     |
| View status                             | `git status`        |                                           |
| View branches                           | `git branch`        |                                           |
|                                         | `-a`                | all branches                              |
|                                         | `-r`                | remote branches                           |
|                                         | `-vv`               | upstream branches and commit message      |
| View commit history                     | `git log`           |                                           |
|                                         | `--oneline`         | print on one line                         |
|                                         | `-n <N>`            | view the last N commits                   |
|                                         | `-p <filename>`     | history + changes to file for each commit |
|                                         | `--follow <file>`   | history beyond renames                    |
| View changes made relative to the index | `git diff`          |                                           |
|                                         | `--cached`          | view staged changes relative to HEAD      |
|                                         | `<commit> <commit>` | view changes between two commits          |

----
## Branches
| Function                                      | Command                   | Option                    |
|-----------------------------------------------|---------------------------|--------------------------:|
| Create branch at HEAD                         | `git branch <name>`       |                           |
|                                               | `<commit#>`               | starting point for branch |
| Switch to branch                              | `git switch <name>`       |                           |
|                                               | `-c`                      | create + switch to branch |
| a) Delete branch                              | `git branch -d <name>`    |                           |
| b) Push to origin to reflect change to branch | `git push origin :<name>` |                           |

----
## Synchronising Changes
| Function                                               | Command     | Option             |
|--------------------------------------------------------|-------------|-------------------:|
| a) Download contents from the remote tracking branches | `git fetch` |                    |
| b) Integrate changes into current local branch         | `git merge` |                    |
| **a+b**                                                | `git pull`  |                    |
|                                                        | `-r`        | fetch + rebase     |
| Rebase                                                 | `git rebase`|                    |
|                                                        | `-i`        | interactive rebase |

----
## Making Changes
| Function                              | Command                      | Option                                   |
|---------------------------------------|----------------------------  |-----------------------------------------:|
| Stage changes                         | `git add`                    |                                          |
|                                       | `-A`                         | all files                                |
|                                       | `-u`                         | update tracked (modified, deleted) files |
| Commit changes                        | `git commit`                 |                                          |
|                                       | `-m 'commit message'`        | with message                             |
| Amend the last commit                 | `git commit --amend`         |                                          |
|                                       | `-m 'new message'`           | edit message                             |
|                                       | `--no-edit`                  | commit without editing message           |
| Transfer commits from local to remote | `git push <remote> <branch>` |                                          |
|                                       | `-u`                         | set remote as upstream                   |

---
## Undoing Changes
| Function                                     | Command               | Option |
|----------------------------------------------|-----------------------|-------:|
| Undo a **local commit** to specified commit  | `git reset <commit>`  |        |
| Undo a **pushed commit** to specified commit | `git revert <commit>` |        |

A new commit is added at the end of the branch to 'cancel' the changes.

The preferred method of undoing shared history is `git revert`. A revert is safer than a reset because it will not remove any commits from a shared history. A revert will retain the commits you want to undo and create a new commit that inverts the undesired commit. This method is safer for shared remote collaboration because a remote developer can then pull the branch and receive the new revert commit which undoes the undesired commit.

(From https://www.atlassian.com/git/tutorials/undoing-changes)

---

## Stash
| Function                          | Command                   | Option                              |
|-----------------------------------|---------------------------|------------------------------------:|
| View stashes                      | `git stash list`          |                                     |
| View list of files changed        | `git stash show`          |                                     |
|                                   | `n`                       | list of files changed in stash{n}   |
|                                   | `-p`                      | patch                               |
| Stash changes (save to a stack)   | `git stash`               |                                     |
|                                   | `-m 'stash-name'`         | with message                        |
|                                   | `--all`                   | include untracked and ignored files |
|                                   | `<filepath1> <filepath2>` | specific files                      |
| Apply and delete last stash entry | `git stash pop`           |                                     |
|                                   | `n`                       | Apply stash{n}                      |
| Apply and keep stash entry        | `git stash apply`         |                                     |
|                                   | `n`                       | Apply stash{n}                      |
| Delete stash                      | `git stash drop`          |                                     |
|                                   | `n`                       | Delete stash{n}                     |
| Delete all stashes                | `git stash clear`         |                                     |

---

## Scenarios
---
### Force `git pull` to overwrite local files
1. Update all `origin/<branch>` refs to latest:    
2. Jump to the latest commit on `origin/master` and checkout the files:

        git fetch --all
        git reset --hard origin/master

### Pop stash without losing uncommitted work
1. Temporarily stage uncommitted changes:
2. Apply stash
- If there are any conflicts, fix them and run `git add .`
3. Unstage everything

        git add -u .
        git stash pop
        (git add .)
        git reset

git fetch origin other-branch
git merge origin/other-branch