# Notes: Basic Git & GitHub Usage

## Clone a Repository

```bash
git clone <repository-url>
cd <repository-name>
```

---

## Check Status

```bash
git status
```

---

## Update Local Repository

```bash
git switch main
git pull
```

---

## Create and Switch to a New Branch

```bash
git switch -c <branch-name>
```

Example:

```bash
git switch -c feature/login
```

---

## Switch Branches

```bash
git switch <branch-name>
```

Example:

```bash
git switch main
```

---

## List Branches

```bash
git branch
```

---

## Add Changes

Add all changes:

```bash
git add .
```

Add one file:

```bash
git add <file>
```

---

## Commit Changes

```bash
git commit -m "commit message"
```

Example:

```bash
git commit -m "Add login feature"
```

---

## Push Changes

First push of a new branch:

```bash
git push -u origin <branch-name>
```

After that:

```bash
git push
```

Push directly to `main`:

```bash
git push origin main
```

---

## Create a Pull Request

If GitHub CLI is installed:

```bash
gh pr create --base main
```

---

## Delete a Branch

Delete a local branch:

```bash
git branch -d <branch-name>
```

Force delete:

```bash
git branch -D <branch-name>
```

Delete a remote branch:

```bash
git push origin --delete <branch-name>
```

---

## Check Commit History

```bash
git log --oneline
```

Press `q` to exit.

---

## Undo the Latest Commit

If the commit has already been pushed:

```bash
git revert HEAD
```

If the commit has not been pushed and you want to keep the changes:

```bash
git reset --soft HEAD~1
```

---

# Typical Workflow

```bash
git switch main
git pull

git switch -c feature/my-feature

# edit files

git status
git add .
git commit -m "Add my feature"
git push -u origin feature/my-feature

gh pr create --base main
```

After the PR is merged:

```bash
git switch main
git pull
git branch -d feature/my-feature
```
