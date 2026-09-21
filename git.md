# Essential Git Commands

## 1. Starting a Project

### `git clone <url>`

Downloads an existing repository from [GitHub](https://github.com/) to your computer.

```bash
git clone <url>
```

### `git init`

Initializes a brand-new, empty local Git repository in your current folder.

```bash
git init
```

---

## 2. Saving Everyday Work

### `git status`

Shows the current state of your folder, including modified, deleted, and untracked files.

```bash
git status
```

### `git add <file>`

Stages specific changes so they can be included in your next commit.

```bash
git add <file>
```

To stage all changes:

```bash
git add .
```

### `git commit -m "your message"`

Permanently saves your staged changes to your local Git history with a descriptive message.

```bash
git commit -m "your message"
```

---

## 3. Syncing with GitHub

### `git push origin <branch-name>`

Uploads your local commits to your GitHub repository.

```bash
git push origin <branch-name>
```

### `git pull`

Downloads and merges changes from GitHub into your local computer. This is especially important when working with a team.

```bash
git pull
```

### `git fetch`

Downloads the latest data from GitHub without automatically merging it into your active files. This allows you to safely review changes first.

```bash
git fetch
```

---

## 4. Inspecting History

### `git log`

Displays the chronological history of commits made in the current branch.

```bash
git log
```

### `git diff`

Shows line-by-line changes that have been made but not yet staged or committed.

```bash
git diff
```
