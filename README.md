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

## 🧠 The four places Git tracks work

Understanding these four places makes the commands easier to remember:

| Place | What it contains | How to inspect it |
| --- | --- | --- |
| Working tree | The files currently on disk, including edits not selected for a commit. | `git status`, `git diff` |
| Staging area (index) | The exact snapshot prepared for the next commit. | `git diff --staged` |
| Local repository | Commits saved on this computer. | `git log` |
| Remote repository | Commits shared with a host such as GitHub. | `git fetch`, `git remote -v` |

The usual path is **edit → stage → commit → push**. `git add` copies selected changes into the index; it does not commit them. `git commit` records the index as a new local snapshot. `git push` sends local commits to a remote. `git fetch` downloads remote commits without changing your current files; `git pull` fetches and integrates them.

## 🎯 Stage only the hunks you want

The staging area is a draft of your *next commit*. It can contain only some files, or even only some lines from a file. This is useful when one file contains two unrelated changes and you want them in separate commits.

### Interactive staging with `git add -p`

Start by inspecting the file and repository:

```bash
git status
git diff
git add -p src/example.js
```

Git shows a **hunk**, a nearby group of added and removed lines, then prompts for an action. The exact prompt can vary slightly by Git version. The common choices are:

| Key | Meaning |
| --- | --- |
| `y` | Stage this entire hunk. |
| `n` | Leave this hunk unstaged and move on. |
| `s` | Split the hunk into smaller hunks when Git can separate the changes. |
| `e` | Edit the patch manually to stage only selected lines. |
| `q` | Quit; keep any hunks already staged in this session. |
| `a` | Stage this and all remaining hunks in this file. |
| `d` | Leave this and all remaining hunks in this file unstaged. |
| `?` | Show the available choices. |

For changes that touch nearby lines, choose `s` first. If Git cannot split the hunk further, choose `e` to edit the patch. In the patch editor, keep the patch headers intact. To leave a changed line out of the staged patch, change its leading `+` or `-` marker to a space; do not delete the line. Save and close the editor to apply the edited patch. If unsure, quit the editor without saving, then review the staged diff.

### Review exactly what will be committed

```bash
git diff --staged   # Only the selected hunks; this is the proposed commit
git diff            # Remaining unstaged edits
```

If you staged the wrong hunk, you can unstage all changes from a file while preserving its working copy:

```bash
git restore --staged src/example.js
```

Then run `git add -p src/example.js` again. You can also reverse a specific staged patch interactively with `git restore --staged -p src/example.js` on current Git versions.

### Commit the selected hunks

```bash
git commit -m "Describe the focused change"
```

The commit contains only the staged snapshot. Any edits left unstaged remain in your working tree for a later commit. After committing, check `git status` and `git diff` so you know what is still pending.

### Interactive staging example

Suppose you changed a function and also added a debug print in the same file. Stage the function change and leave the print for later:

```bash
git add -p src/example.js
# At the function hunk: y (or s, then y for the intended smaller hunk)
# At the debug-print hunk: n
git diff --staged
git diff
git commit -m "Fix example calculation"
```

The first diff should contain the function fix only. The second diff should still show the debug print. This is called a **partial commit**: the index captures a deliberate subset of the working tree.

## 🧩 Stage and unstage at different levels

| Goal | Command | Result |
| --- | --- | --- |
| Stage one path | `git add path/to/file` | All current changes to that path enter the index. |
| Stage all changes, including removals | `git add -A` | Stages tracked and untracked changes across the repository. |
| Stage changes under this directory | `git add .` | Stages changes beneath the current directory. |
| Stage only selected hunks | `git add -p` | Prompts for each hunk. |
| Unstage one path, keep edits | `git restore --staged path/to/file` | Removes that path's changes from the index. |
| Unstage selected hunks | `git restore --staged -p path/to/file` | Interactively removes hunks from the index. |
| Discard unstaged edits | `git restore path/to/file` | Replaces the working copy with the indexed version; destructive. |
| Discard all unstaged edits | `git restore .` | Replaces working files with indexed versions; destructive. |

`git add .` is convenient but can stage unrelated work. For more control, name files explicitly or use `git add -p`, then always inspect `git diff --staged` before committing.

## 🔀 Merge conflicts

A conflict means Git could not decide how to combine edits to the same area. Git marks the affected file and pauses the merge; the file contains markers such as:

```text
<<<<<<< HEAD
current branch's version
=======
incoming branch's version
>>>>>>> feature/example
```

1. Run `git status` to find every conflicted path.
2. Open each file and choose or combine the correct lines.
3. Remove all conflict marker lines and save the file.
4. Review the resolved file with `git diff`.
5. Mark it resolved and stage the chosen result: `git add <file>`.
6. Review the merge result with `git diff --staged`.
7. Finish with `git commit` if Git has not already created the merge commit.

To abandon an unfinished merge and return to its pre-merge state, use `git merge --abort`. This is different from fixing a conflict: abort only when you want to cancel the merge.

## 🪄 Rebase and clean up local history

A rebase replays commits from your branch on top of another base. It can make a feature branch easier to review, but it creates new commit IDs. Do not rebase commits other people already use unless your team has agreed to rewrite that history.

```bash
git fetch origin
git switch feature/example
git rebase origin/main
```

If a conflict occurs, resolve the file, stage it, and continue:

```bash
git status
git add <resolved-file>
git rebase --continue
```

Use `git rebase --abort` to return to the pre-rebase state, or `git rebase --skip` only when you intentionally want to omit the current commit.

Interactive rebase is useful for cleaning up your **own unpublished commits**:

```bash
git rebase -i HEAD~4
```

The editor lists the four commits oldest first. Common actions include `pick` (keep), `reword` (change message), `edit` (pause to amend), `squash` (combine commits and edit the combined message), `fixup` (combine and discard the later message), and `drop` (remove a commit). Save and close to start. Resolve any conflicts and use `git rebase --continue`; use `git rebase --abort` to cancel.

## 🧳 Temporarily set changes aside with stash

A stash lets you switch tasks without committing unfinished work. By default, untracked files are not included; add `-u` to include them.

```bash
git stash push -m "unfinished search work"     # Save tracked edits
git stash push -u -m "search work with new files" # Include untracked files
git stash list                                  # Find saved stashes
git stash show -p stash@{0}                     # Inspect one stash
git stash apply stash@{0}                       # Reapply and keep the stash
git stash pop                                  # Reapply and remove the newest stash
git stash drop stash@{0}                        # Remove one stash after checking it
git stash clear                                # Remove every stash; use carefully
```

`apply` keeps the saved copy, while `pop` removes it after applying successfully. A stash is local temporary storage, not a backup or a substitute for a commit.

## 🧹 Ignore files and stop tracking generated or secret files

Add patterns to `.gitignore` at the repository root. For example:

```gitignore
.env
.venv/
__pycache__/
*.py[cod]
node_modules/
build/
.DS_Store
```

`.gitignore` prevents matching **untracked** files from being added by default; it does not remove a file already committed or tracked. To stop tracking a file while keeping the local copy:

```bash
git rm --cached .env
git commit -m "Stop tracking local environment file"
```

Check which ignore rule applies with `git check-ignore -v path/to/file`. If a secret was committed, removing it in a later commit does not erase it from history; rotate the exposed credential and follow your host's guidance for removing sensitive data from history.

## 🔎 Search commits and find regressions

```bash
git log --grep="authentication" --oneline  # Search commit messages
git log -S "calculateTotal" --oneline       # Find commits adding/removing this exact string
git log -G "calculate.*Total" --oneline     # Search changed lines with a regular expression
git blame -L 50,80 src/example.js             # See the last commit for lines 50–80
```

`git bisect` performs a binary search through history to locate the commit that introduced a bug. You need a known-good commit and a commit where the bug exists:

```bash
git bisect start
git bisect bad                 # Current checkout has the bug
git bisect good <known-good-commit>
# Test the checked-out revision, then tell Git:
git bisect good                # or: git bisect bad
git bisect reset               # Return to the original branch and revision
```

Repeat the test and `good`/`bad` marking until Git identifies the first bad commit. If the test can be automated, `git bisect run <test-command>` can perform the search for you.

## 🏷️ Tags, remote branches, and tracking

Tags are names for particular commits, commonly used for releases. Annotated tags include metadata and are usually preferred for published releases.

```bash
git tag -a v1.0.0 -m "Release 1.0.0"  # Create an annotated tag
git show v1.0.0                        # Inspect it
git push origin v1.0.0                 # Publish one tag
git tag -d v1.0.0                       # Delete a local tag
git push origin --delete v1.0.0         # Delete a remote tag
```

A **remote-tracking branch** such as `origin/main` is your local record of a branch last seen on the remote. `git fetch` updates that record. A local branch can track a remote branch so `git pull` and `git push` know their default destination.

```bash
git branch -vv                               # Show branches and upstreams
git switch -c feature/example                # Create a local branch
git push -u origin feature/example           # Publish it and set its upstream
git branch --set-upstream-to=origin/main main # Set upstream explicitly
git fetch --prune                             # Fetch and remove stale remote refs
git push origin --delete feature/example     # Delete a remote branch
```

## 🧰 Worktrees and aliases

A worktree creates another working directory connected to the same local repository. This lets you check out a second branch in a separate folder without stashing your current changes.

```bash
git worktree add ../hotfix hotfix/login       # Add a worktree for an existing branch
git worktree add -b feature/new ../new-feature # Create a branch and its worktree
git worktree list                              # List worktrees
git worktree remove ../hotfix                  # Remove a clean worktree
```

Git aliases shorten frequent commands. They are optional and only affect your configuration:

```bash
git config --global alias.st status
git config --global alias.co switch
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Then use `git st` or `git lg`. Learn the full command first so aliases do not hide what Git is doing.

## 📈 A practical learning order

1. **Daily essentials:** `status`, `diff`, `add`, `commit`, `log`, `switch`, `branch`.
2. **Collaboration:** `fetch`, `pull`, `push`, `remote`, `merge`.
3. **Careful recovery:** `restore`, `revert`, `reset`, `stash`, `reflog`.
4. **History and debugging:** `rebase`, `rebase -i`, `cherry-pick`, `bisect`.
5. **Useful extras:** `tag`, `clean`, `worktree`, `add -p`.

You do not need to memorize every option. Build the habit of checking `git status`, reviewing `git diff` and `git diff --staged`, and understanding whether a command changes the working tree, index, local commits, or remote repository.
