# Git Command Handbook

<div align="center">

**A practical, copy-ready reference for the Git commands you will use most.**

![Git](https://img.shields.io/badge/Git-version%20control-F05032?logo=git&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Contributions](https://img.shields.io/badge/Contributions-welcome-brightgreen.svg)

[Getting started](#-getting-started) · [Daily workflow](#-the-daily-workflow) · [Command guide](#-command-guide) · [Undo safely](#-undo-safely)

</div>

Git is a version control system that records changes to files. This handbook covers the everyday workflow, collaboration, recovery, and useful inspection commands in one place.

> **Quick rule:** inspect with `git status` before changing history. Prefer `git revert` for changes already shared with others.

## ✨ Getting started

```bash
# Check your installed Git version
git --version

# Set the name and email recorded in your commits (once per computer)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Choose a default branch name for new repositories
git config --global init.defaultBranch main

# Start tracking a folder, or copy an existing remote repository
git init
git clone https://github.com/OWNER/REPOSITORY.git
```

## 🔁 The daily workflow

```bash
git status                     # See changed, staged, and untracked files
git add README.md              # Stage one file
git add .                      # Stage changes under the current directory
git diff                       # Review unstaged changes
git diff --staged              # Review what will go into the commit
git commit -m "Add setup guide" # Save staged changes
git push                       # Send commits to the remote
```

**A good habit:** run `git diff --staged` before every commit. It helps catch accidental files and secrets.

## 📚 Command guide

<details>
<summary><strong>1. Start a repository and configure Git</strong></summary>

| Command | What it does |
| --- | --- |
| `git --version` | Prints the installed version. |
| `git config --list` | Shows effective Git settings. |
| `git config --global user.name "Your Name"` | Sets the author name for your commits. |
| `git config --global user.email "you@example.com"` | Sets the author email for your commits. |
| `git config --global init.defaultBranch main` | Uses `main` for newly initialized repositories. |
| `git init` | Creates a repository in the current folder. |
| `git clone <url>` | Downloads a repository and its history. |
| `git clone <url> <folder>` | Downloads it into a chosen folder. |

</details>

<details>
<summary><strong>2. Inspect changes and history</strong></summary>

| Command | What it does |
| --- | --- |
| `git status` | Summarizes the repository state. Run it often. |
| `git status -sb` | Shows a compact status and current branch. |
| `git diff` | Shows unstaged changes. |
| `git diff --staged` | Shows staged changes. |
| `git diff <branch>...HEAD` | Shows changes on your branch since it diverged. |
| `git log --oneline` | Shows a compact commit history. |
| `git log --oneline --graph --decorate --all` | Draws a compact graph of all branches. |
| `git show <commit>` | Shows a commit and its patch. |
| `git blame <file>` | Shows who last changed each line. |
| `git ls-files` | Lists files tracked by Git. |

</details>

<details>
<summary><strong>3. Stage and commit</strong></summary>

| Command | What it does |
| --- | --- |
| `git add <file>` | Stages one file. |
| `git add .` | Stages changes below the current directory. |
| `git add -p` | Reviews and stages changes piece by piece. |
| `git commit -m "message"` | Creates a commit from staged changes. |
| `git commit` | Opens your editor to write a commit message. |
| `git commit --amend` | Edits the most recent commit. Avoid after pushing it. |
| `git commit --amend --no-edit` | Adds staged changes to the last commit, keeping its message. |
| `git restore --staged <file>` | Unstages a file while keeping its edits. |
| `git restore <file>` | Discards unstaged edits to a file. **This is destructive.** |

</details>

<details>
<summary><strong>4. Branches and merging</strong></summary>

| Command | What it does |
| --- | --- |
| `git branch` | Lists local branches. |
| `git branch -a` | Lists local and remote-tracking branches. |
| `git switch -c <name>` | Creates and switches to a branch. |
| `git switch <name>` | Switches to an existing branch. |
| `git branch -d <name>` | Deletes a merged local branch. |
| `git merge <branch>` | Merges a branch into the current branch. |
| `git merge --abort` | Cancels an in-progress merge. |
| `git mergetool` | Opens the configured conflict resolution tool. |
| `git branch -m <new-name>` | Renames the current branch. |

Common pattern:

```bash
git switch main
git pull --ff-only
git switch -c feature/short-description
# make changes, stage, and commit
git switch main
git merge feature/short-description
```

</details>

<details>
<summary><strong>5. Work with remotes and collaborate</strong></summary>

| Command | What it does |
| --- | --- |
| `git remote -v` | Shows configured remotes and URLs. |
| `git remote add origin <url>` | Adds a remote named `origin`. |
| `git remote set-url origin <url>` | Changes the URL for `origin`. |
| `git fetch` | Downloads remote updates without merging them. |
| `git fetch --prune` | Fetches updates and removes stale remote-tracking refs. |
| `git pull` | Fetches and integrates updates from the upstream branch. |
| `git pull --ff-only` | Updates only when the branch can fast-forward. |
| `git push -u origin <branch>` | Publishes a branch and sets its upstream. |
| `git push` | Pushes the current branch to its upstream. |
| `git push --force-with-lease` | Safely force-pushes when the remote is as expected. Use carefully. |

</details>

<details>
<summary><strong>6. Undo and recover safely</strong></summary>

| Command | What it does |
| --- | --- |
| `git restore <file>` | Discards unstaged file edits. **Cannot be easily recovered.** |
| `git restore --staged <file>` | Unstages a file but preserves its edits. |
| `git revert <commit>` | Creates a new commit that reverses an earlier commit. Safe for shared history. |
| `git reset --soft HEAD~1` | Removes the last commit but keeps changes staged. |
| `git reset --mixed HEAD~1` | Removes the last commit and keeps changes unstaged. |
| `git reset --hard <commit>` | Resets files and history. **Discards work; use with care.** |
| `git reflog` | Lists recent movements of `HEAD`, useful for recovery. |
| `git stash push -m "message"` | Temporarily shelves tracked changes. |
| `git stash list` | Lists saved stashes. |
| `git stash pop` | Reapplies and removes the latest stash. |
| `git stash apply stash@{0}` | Reapplies a stash without removing it. |

If you have already shared a commit, `git revert` is usually the right choice. Rewriting published history with `reset` or force push can disrupt collaborators.

</details>

<details>
<summary><strong>7. Tags, cleanup, and helpers</strong></summary>

| Command | What it does |
| --- | --- |
| `git tag` | Lists tags. |
| `git tag -a v1.0.0 -m "Release 1.0.0"` | Creates an annotated release tag. |
| `git push origin v1.0.0` | Pushes one tag. |
| `git push origin --tags` | Pushes all tags. |
| `git rm <file>` | Removes a tracked file and stages the removal. |
| `git mv <old> <new>` | Moves or renames a tracked file. |
| `git clean -nd` | Previews untracked files that `git clean` would remove. |
| `git clean -fd` | Removes untracked files and folders. **Destructive.** |
| `git cherry-pick <commit>` | Applies one commit to the current branch. |
| `git archive --format=zip HEAD -o source.zip` | Exports the current commit as a ZIP. |

</details>

## 🛡️ Undo safely

| Situation | Recommended command |
| --- | --- |
| I staged the wrong file | `git restore --staged <file>` |
| I want to discard local edits | `git restore <file>` *(permanent)* |
| I want to undo a commit that is already shared | `git revert <commit>` |
| I want to redo my last local commit | `git reset --soft HEAD~1` |
| I cannot find a recent commit | `git reflog` |

## 🌿 A simple branch workflow

1. Update your base branch: `git switch main && git pull --ff-only`.
2. Create a focused branch: `git switch -c feature/add-search`.
3. Make a small change and inspect it: `git status` and `git diff`.
4. Stage and review: `git add <file>` then `git diff --staged`.
5. Commit with a clear message: `git commit -m "Add search filters"`.
6. Push the branch: `git push -u origin feature/add-search`.
7. Open a pull request on your Git hosting site.

## 💡 Helpful tips

- Add generated files, local settings, and secrets to `.gitignore`.
- Never commit passwords, API keys, private keys, or `.env` files.
- Use `git add -p` when you want to commit only part of a file.
- A commit message should describe the change: `Fix login redirect` is clearer than `updates`.
- Git records snapshots. A branch is a movable name for a line of work.

## 📖 Learn more

- [Official Git documentation](https://git-scm.com/doc)
- [Pro Git book](https://git-scm.com/book/en/v2)
- [GitHub Skills](https://skills.github.com/)

## 📄 License

This handbook is available under the [MIT License](LICENSE).
