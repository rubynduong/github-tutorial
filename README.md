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
| Function                                                         | Command             | Option                                    |
|------------------------------------------------------------------|--------------       |-------------------------------------:     |
| View status (difference between current branch and its upstream) | `git status`        |                                           |
| View branches                                                    | `git branch`        |                                           |
|                                                                  | `-a`                | all branches                              |
|                                                                  | `-r`                | remote branches                           |
|                                                                  | `-vv`               | upstream branches and commit message      |
| View commit history                                              | `git log`           |                                           |
|                                                                  | `--oneline`         | print on one line                         |
|                                                                  | `-n <N>`            | view the last N commits                   |
|                                                                  | `-p <filename>`     | history + changes to file for each commit |
|                                                                  | `--follow <file>`   | history beyond renames                    |
| View changes made relative to the index                          | `git diff`          |                                           |
|                                                                  | `--cached`          | view staged changes relative to HEAD      |
|                                                                  | `<commit> <commit>` | view changes between two commits          |

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
| Function                                               | Command                | Option                                                                    |
|--------------------------------------------------------|---------------------   |--------------------------------------------------------------------------:|
| a) Download contents from all remote tracking branches | `git fetch`            |                                                                           |
|                                                        | `origin <branch>`      | fetch `branch` from origin                                                |
| b) Integrate changes into current local branch         | `git merge`            |                                                                           |
|                                                        | `--abort`              | abort merge                                                               |
|                                                        | `<remote>/<branch>`    | merge remote `branch` into current branch                                 |
| **a+b**                                                | `git pull`             | `git pull origin`                                                         |
|                                                        | `-r`                   | fetch + rebase                                                            |
|                                                        | `--rebase --autostash` | stash local changes, fetch from remote, rebase on top of it and pop stash |
|                                                        | `origin <branch>`      | merge remote `branch` into current branch                                 |
| Rebase                                                 | `git rebase`           |                                                                           |
|                                                        | `-i`                   | interactive rebase                                                        |

Merge vs Rebase: https://www.atlassian.com/git/tutorials/merging-vs-rebasing

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
|                                       | `-u`                         | set remote/branch as upstream            |

---
## Undoing Changes (work in progress)
| Function                                     | Command               | Option |
|----------------------------------------------------|-----------------------|-------:|
| Undo a commit by removing the commit (local commit)  | `git reset`  |        |
|       | `--soft <commit>` | move the branch name; does not change the index; does not change the working tree |
|       | `--soft HEAD~1` | move committed files back to the staging area from the previous commit without removing their changes |
|       | `--mixed <commit>` | move the branch name; reset the index to the commit; does not change the working tree |
|       | `--hard <commit>` | move the branch name; reset the index to the commit; reset the working tree |
| Restore a file with its version from another commit | `git restore` |        |
|       | `--file` | restore file from  index / discard changes |
|       | `--staged -- file` | unstage a staged file |
|       | `--source <commit> --staged -- file` | copy file from commit to index without touching the copy in working tree |
| Invert the changes in a commit by creating an additional commit (pushed commit) | `git revert <commit>` |        |

`git reset` vs `git revert`
1. `git revert` undoes a single commit at any point in history, while `git reset` removes all commits that occurred after the target commit.
2. `git revert` does not change project history, which is safe (for collaboration) for commits that have been pushed to the remote.

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
### Resolve merge conflicts
You can do one of two things:
1. Decide not to merge: `git merge --abort`
2. Resolve conflicts by using an editor. Then do:

        git add <file>
        git merge --continue
You can also work through conflicts with `git mergetool` and `git diff`.

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