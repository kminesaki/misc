# Notes: Basic Git & GitHub Usage

This memo covers common Git and GitHub commands for everyday development.

> Note: Most modern repositories use `main` as the default branch.  
> Some older repositories may still use `master`.

---

## 1. Check the Current Status

Before making changes, check the current state of your repository:

```bash
git status
```

This shows:

- which branch you are on
- modified files
- staged files
- untracked files
- whether your branch is ahead of or behind the remote branch

---

## 2. Update Your Local Repository

Before starting new work, it is usually a good idea to update your local `main` branch.

```bash
git switch main
git pull origin main
```

Short version, if your local branch is already tracking the remote branch:

```bash
git pull
```

---

## 3. Basic Workflow: Add → Commit → Push

### Step 1: Check changes

```bash
git status
```

To see the actual content of unstaged changes:

```bash
git diff
```

---

### Step 2: Add changes

Add all changed and new files:

```bash
git add .
```

Add a specific file:

```bash
git add path/to/file
```

Example:

```bash
git add README.md
```

Check what is staged:

```bash
git status
```

---

### Step 3: Commit changes

```bash
git commit -m "Describe the changes"
```

Example:

```bash
git commit -m "Add login form"
```

A commit saves a snapshot of the staged changes in your local Git history.

---

### Step 4: Push changes

Push the current branch:

```bash
git push
```

For the first push of a new branch:

```bash
git push -u origin <branch-name>
```

Example:

```bash
git push -u origin feature/login
```

The `-u` option sets the upstream branch, so future pushes can usually be done with:

```bash
git push
```

To push directly to `main`:

```bash
git push origin main
```

> In many team projects, direct pushes to `main` are disabled.  
> In that case, create a branch and open a Pull Request instead.

---

# Branch Management

## 4. List Branches

Show local branches:

```bash
git branch
```

Show local and remote branches:

```bash
git branch -a
```

---

## 5. Create a New Branch

Create and switch to a new branch:

```bash
git switch -c <branch-name>
```

Example:

```bash
git switch -c feature/login
```

Older equivalent:

```bash
git checkout -b feature/login
```

---

## 6. Switch Branches

```bash
git switch <branch-name>
```

Example:

```bash
git switch main
```

Older equivalent:

```bash
git checkout main
```

---

## 7. Delete a Branch

Delete a local branch after it has been merged:

```bash
git branch -d <branch-name>
```

Example:

```bash
git branch -d feature/login
```

Force-delete a local branch:

```bash
git branch -D <branch-name>
```

Delete a remote branch from GitHub:

```bash
git push origin --delete <branch-name>
```

Example:

```bash
git push origin --delete feature/login
```

You cannot delete the branch you are currently on, so switch to another branch first:

```bash
git switch main
git branch -d feature/login
```

---

## 8. Fetch Remote Branches

Update your local information about remote branches:

```bash
git fetch origin
```

Then list all branches:

```bash
git branch -a
```

To create a local branch from a remote branch:

```bash
git switch --track origin/<branch-name>
```

Example:

```bash
git switch --track origin/feature/login
```

You can also use:

```bash
git switch -c feature/login origin/feature/login
```

---

# Clone a Repository

## 9. Clone a Remote Repository to Your Computer

### Step 1: Get the repository URL

On GitHub, open the repository and copy either:

- the SSH URL, or
- the HTTPS URL

Example SSH URL:

```text
git@github.com:username/repository.git
```

Example HTTPS URL:

```text
https://github.com/username/repository.git
```

### Step 2: Move to the parent directory

For example:

```bash
cd ~/Documents
```

You normally do **not** need to create the repository folder yourself.  
`git clone` will create it automatically.

### Step 3: Clone

```bash
git clone <repository-url>
```

Example:

```bash
git clone git@github.com:username/repository.git
```

Then move into the cloned repository:

```bash
cd repository
```

---

# Pull and Fetch

## 10. `git pull`

Download remote changes and integrate them into your current branch:

```bash
git pull
```

Explicit version:

```bash
git pull origin main
```

Conceptually:

```text
git pull = git fetch + integrate remote changes
```

---

## 11. `git fetch`

Download information from the remote repository without modifying your working files:

```bash
git fetch origin
```

This is useful when you want to inspect remote changes before merging them.

---

# Commit History

## 12. View Commit History

Compact history:

```bash
git log --oneline
```

Example output:

```text
a1b2c3d Add login form
d4e5f6g Update README
h7i8j9k Initial commit
```

Press:

```text
q
```

to exit the log viewer.

A more visual version:

```bash
git log --oneline --graph --decorate --all
```

---

# Undo Changes

## 13. Undo Uncommitted Changes

Discard changes to a specific file:

```bash
git restore <file>
```

Example:

```bash
git restore README.md
```

Discard all unstaged changes:

```bash
git restore .
```

> Warning: these changes will be lost.

---

## 14. Unstage a File

If you accidentally ran `git add`:

```bash
git restore --staged <file>
```

Example:

```bash
git restore --staged README.md
```

The file remains modified, but it is removed from the staging area.

---

## 15. Undo a Commit Safely

To reverse an existing commit while keeping the Git history:

```bash
git revert <commit-hash>
```

To reverse the latest commit:

```bash
git revert HEAD
```

Find commit hashes with:

```bash
git log --oneline
```

Example:

```bash
git log --oneline
git revert a1b2c3d
```

`git revert` creates a new commit that reverses the selected commit.  
This is generally safer for commits that have already been pushed.

---

## 16. Undo the Latest Local Commit

If the commit has **not** been pushed yet, you may use `git reset`.

Keep the changes staged:

```bash
git reset --soft HEAD~1
```

Keep the changes but unstage them:

```bash
git reset HEAD~1
```

Delete the commit and its changes completely:

```bash
git reset --hard HEAD~1
```

> Be careful with `--hard`: it permanently discards local changes.

---

# Remote Repository Information

## 17. Check Remote URLs

```bash
git remote -v
```

Example:

```text
origin  git@github.com:username/repository.git (fetch)
origin  git@github.com:username/repository.git (push)
```

---

# Pull Requests with GitHub CLI

If GitHub CLI (`gh`) is installed, you can create and manage Pull Requests from the terminal.

## 18. Create a Pull Request

Typical workflow:

```bash
git switch main
git pull
git switch -c feature/new-feature

# edit files

git add .
git commit -m "Add new feature"
git push -u origin feature/new-feature
gh pr create
```

Create a PR targeting `main`:

```bash
gh pr create --base main
```

Create a PR with a title and body:

```bash
gh pr create \
  --base main \
  --title "Add new feature" \
  --body "This PR adds the new feature."
```

---

## 19. Useful Pull Request Commands

List Pull Requests:

```bash
gh pr list
```

View the current PR:

```bash
gh pr view
```

Open the PR in a browser:

```bash
gh pr view --web
```

Check out a Pull Request locally:

```bash
gh pr checkout <PR-number>
```

Example:

```bash
gh pr checkout 123
```

Merge a Pull Request:

```bash
gh pr merge <PR-number>
```

---

# Typical Daily Workflow

A common Git workflow looks like this:

```bash
# 1. Go to main
git switch main

# 2. Get the latest changes
git pull

# 3. Create a new branch
git switch -c feature/my-feature

# 4. Edit files

# 5. Check changes
git status
git diff

# 6. Stage changes
git add .

# 7. Commit
git commit -m "Add my feature"

# 8. Push the branch
git push -u origin feature/my-feature

# 9. Create a Pull Request
gh pr create --base main
```

After the PR has been merged:

```bash
git switch main
git pull
git branch -d feature/my-feature
```

If you also want to remove the remote branch manually:

```bash
git push origin --delete feature/my-feature
```

---

# Quick Command Cheat Sheet

| Purpose | Command |
|---|---|
| Check status | `git status` |
| View changes | `git diff` |
| Add all changes | `git add .` |
| Add one file | `git add <file>` |
| Commit | `git commit -m "message"` |
| Push | `git push` |
| First push of branch | `git push -u origin <branch>` |
| Pull latest changes | `git pull` |
| Fetch remote information | `git fetch origin` |
| List local branches | `git branch` |
| List all branches | `git branch -a` |
| Create + switch branch | `git switch -c <branch>` |
| Switch branch | `git switch <branch>` |
| Delete local branch | `git branch -d <branch>` |
| Force-delete local branch | `git branch -D <branch>` |
| Delete remote branch | `git push origin --delete <branch>` |
| Clone repository | `git clone <url>` |
| View commit history | `git log --oneline` |
| View remote URLs | `git remote -v` |
| Undo a pushed commit safely | `git revert <commit>` |
| Unstage a file | `git restore --staged <file>` |
| Discard local file changes | `git restore <file>` |
| Create Pull Request | `gh pr create` |
| List Pull Requests | `gh pr list` |

---

# Useful Mental Model

The basic Git flow is:

```text
Working Directory
      |
      | git add
      v
Staging Area
      |
      | git commit
      v
Local Repository
      |
      | git push
      v
GitHub / Remote Repository
```

To get changes from GitHub:

```text
GitHub / Remote Repository
      |
      | git pull
      v
Local Repository + Working Directory
```

---

# Reference

- [Atlassian Git Tutorial: Undoing Commits and Changes](https://www.atlassian.com/git/tutorials/undoing-changes)
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com/)
