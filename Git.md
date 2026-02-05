# Git (Distributed Version Control System)

## Git Branching Strategies

1. **Master/Main**: The up-to-date code branch where the latest production-ready code resides.
2. **Feature Branch**: Developers actively work on this branch to make changes or develop new features.
3. **Hot-line/Bug-fix**: Used for urgent fixes when issues arise. This branch is merged into the `master` and released immediately.
4. **Release Branch**: This branch is used for delivering app code to build and test. It keeps the `master` branch up-to-date for future releases.

---

## Basic Git Commands

- **`git init`**: Initializes a new Git repository.
- **`git clone <repository-url>`**: Clones an existing repository into a new directory.
- **`git add <file>`**: Stages changes in the specified file for the next commit.
- **`git add .`**: Stages all changes in the current directory. Reviewing the changes.
- **`git commit -m "message"`**: Commits the staged changes with a descriptive message.
- **`git status`**: Shows the status of changes in the working directory and staging area.
- **`git log`**: Displays the commit history.

---

## Branching

- **`git branch`**: Lists all branches in the repository.
- **`git branch <branch-name>`**: Creates a new branch.
- **`git checkout <branch-name>`**: Switches to the specified branch.
- **`git checkout -b <branch-name>`**: Creates and switches to a new branch.
- **`git merge <branch-name>`**: Merges the specified branch into the current branch.
  - 1️⃣ Fast-forward merge
    - Happens when main has no new commits after branch creation.
      ```
      git checkout main
      git merge feature/login
      ```
      - No merge commit
      - Pointer just moves forward
        ```
        A──B──C──D
        ```
- ✔️ Clean history
- ❌ Loses info that a branch existed

  - 2️⃣ Three-way merge (normal merge)
    - Happens when both branches have new commits.
    - Git creates a merge commit.
    ```
        A──B───M
       └──C──D
    ```
- ✔️ Preserves branch history
- ✔️ Preferred in teams

- git merge integrates changes from one branch into another. It can result in a fast-forward merge or a three-way merge. If the same code is changed in both branches, Git raises a merge conflict which must be resolved manually.
---

## Remote Repositories

- **`git remote add <name> <url>`**: Adds a new remote repository.
- **`git remote -v`**: Lists the configured remote repositories.
- **`git push <remote> <branch>`**: Pushes commits to the specified branch of the remote repository.
- **`git pull <remote> <branch>`**: Fetches and merges changes from the remote repository into the current branch.
- **`git fetch <remote>`**: Downloads commits, files, and refs from a remote repository.

---

## Undoing Changes

- **`git checkout -- <file>`**: Discards changes in the specified file.
- **`git reset <file>`**: Unstages a staged file.
- **`git reset --hard`**: Resets the working directory to the last commit, discarding all changes.

---

## Viewing Changes

- **`git diff`**: Shows changes between the working directory and the index (staging area).
- **`git diff --staged`**: Shows changes between the index and the last commit.

---

## Tagging

- **`git tag <tag-name>`**: Creates a tag at the current commit.
- **`git tag -a <tag-name> -m "message"`**: Creates an annotated tag.

---

## Configuration

- **`git config --global user.name "Your Name"`**: Sets the global username for commits.
- **`git config --global user.email "your.email@example.com"`**: Sets the global email for commits.

---

## Stashing

- **`git stash`**: Saves your uncommitted changes and reverts to the last commit.
- **`git stash pop`**: Applies the latest stash and removes it from the stash list.
- **`git stash list`**: Lists all stashed changes.
- **`git stash apply`**: Applies the latest stash but keeps it in the stash list.

---

## Rebase

- **`git rebase <branch>`**: Applies your changes on top of another branch.
- **`git rebase -i <commit>`**: Starts an interactive rebase from the specified commit.

---

## Cherry Picking

- **`git cherry-pick <commit>`**: Applies a specific commit from another branch to the current branch.

---

## Viewing History

- **`git log --oneline`**: Displays a compact view of the commit history.
- **`git log --graph --decorate --oneline`**: Visual representation of the commit history, showing branches and tags.

---

## Diffing

- **`git diff <commit1> <commit2>`**: Shows the differences between two commits.
- **`git diff <branch1>..<branch2>`**: Compares changes between two branches.

---

## Resetting

- **`git reset <commit>`**: Moves the current branch pointer to a specific commit. Use `--soft`, `--mixed`, or `--hard` to change how changes are handled.
  - **`--soft`**: Keeps changes staged.
  - **`--mixed`**: Keeps changes unstaged.
  - **`--hard`**: Discards changes.

---

## Cleaning Up

- **`git clean -f`**: Removes untracked files from the working directory.
- **`git clean -fd`**: Removes untracked files and directories.

---

## Configurations

- **`git config --list`**: Lists all the configurations.
- **`git config --global core.editor <editor>`**: Sets the default text editor for Git (e.g., nano, vim, etc.).

---

## Viewing and Searching

- **`git show <commit>`**: Displays details about a specific commit.
- **`git blame <file>`**: Shows who changed each line of a file and when.

---

## Workflow

- **`git tag -d <tag-name>`**: Deletes a local tag.
- **`git push --tags`**: Pushes all tags to the remote repository.
- **`git reflog`**: Shows a log of all actions that have changed the current branch (useful for recovery).

---

## Working with Submodules

- **`git submodule add <repository-url>`**: Adds a new submodule to your project.
- **`git submodule update --init`**: Initializes and updates your submodules.

---

## `git rm --cached <file>`

- **Purpose**: Removes the specified file from the staging area without deleting it from your working directory. This means the file will not be included in the next commit, but it will still exist in your project.
- **Common Use Case**: This is often used when you accidentally added a file (like a configuration file or sensitive information) that you don’t want to track in your repository.

---

## Creating a New Repository on the Command Line

```bash
echo "# git-repo" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/satishdevops777/git-repo.git
git push -u origin main
```
## Pushing an Existing Repository from the Command Line
```bash
git remote add origin https://github.com/satishdevops777/git-repo.git
git branch -M main
git push -u origin main
```

## Git Clone

**Definition**: git clone is a command that creates a local copy of a repository from a remote source (like GitHub, GitLab, etc.).

**Usage**: When you run git clone <repo_url>, you download all the files, history, and branches of that repository to your local machine.

**Purpose**: It's typically used when you want to work on a project locally, whether it’s your own or someone else's. You directly interact with the original repository.

## Fork

**Definition**: A fork is a copy of a repository that is created on the hosting service (e.g., GitHub) under your account.

**Usage**: When you click the "Fork" button on a repository page, it creates a new repository in your account, allowing you to make changes independently of the original repository.

**Purpose**: Forks are commonly used in open-source projects. After forking, you can clone your fork to your local machine, make changes, and then submit a pull request to the original repository if you want to contribute back.

## Summary
**Clone**: Local copy of any repository (yours or someone else's).

**Fork**: Your own copy of someone else's repository on the hosting service, allowing for independent changes and contributions.
