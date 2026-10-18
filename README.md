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

## 🧱 Hunks, explained in detail

A **hunk** is one section of a diff: a header followed by nearby removed and added lines. A diff hunk often looks like this:

```diff
@@ -10,4 +10,5 @@ function calculateTotal(items) {
   const subtotal = sum(items);
-  return subtotal;
+  const tax = subtotal * taxRate;
+  return subtotal + tax;
 }
```

The header `@@ -10,4 +10,5 @@` says the old version shows four lines starting at line 10 and the new version shows five lines starting at line 10. A line beginning with a space is unchanged context; `-` is removed from the old version; `+` is added in the new version. The context helps Git locate the edit when applying it.

Git groups nearby changed lines into one hunk. Two edits may appear together when they are close enough that their surrounding context overlaps. If the changes are far apart, Git usually presents separate hunks. Hunk boundaries are a diff display choice; they do not necessarily correspond to whole functions, features, or commits.

### Why stage by hunk?

The working tree is your full in-progress version of the files. The index is a separate snapshot that you assemble for the next commit. Partial staging lets the index contain only some edits from a file, while the rest stays in the working tree. This makes it possible to create focused commits without first rearranging code or making temporary copies.

For example, if one file contains a bug fix and an unrelated formatting change, you can stage the bug fix hunk, commit it, and leave the formatting change unstaged for a later commit. A commit records the index, not every edit currently visible in your editor.

### Prepare before choosing hunks

```bash
git status --short
git diff -- src/example.js
git add -p -- src/example.js
```

Review the full unstaged diff first so you know which edits exist. Supplying a path limits the interactive session to that file. You can run `git add -p` without a path to review changes across files. If changes are already staged and you run `git add -p` again, Git interactively stages the remaining unstaged changes; it does not remove existing staged content.

### Read the patch prompt

Git prints a patch hunk and asks what to do. Use `?` to show the choices supported by your installed Git version. Common choices are:

| Key | Effect on the current hunk |
| --- | --- |
| `y` | Add the entire displayed hunk to the index. |
| `n` | Leave it out of the index and continue. The edits remain in the working tree. |
| `s` | Split it into smaller hunks, if Git can separate the changes. Git then prompts for each smaller piece. |
| `e` | Open the patch in an editor so you can adjust which changed lines are staged. |
| `q` | Stop the session. Changes already staged during this session stay staged. |
| `a` | Stage this hunk and every later hunk in this file. |
| `d` | Skip this hunk and every later hunk in this file. |
| `?` | Print the interactive help. |

When a hunk contains multiple nearby changes, try `s` first. If Git says the hunk cannot be split further, its changed lines may be too close or the context may overlap. Then either stage the whole hunk with `y`, leave it with `n`, or edit it with `e`.

### Split a hunk into smaller pieces

Suppose one displayed hunk contains a correctness fix and a nearby debug statement. At the hunk prompt, enter:

```text
s
```

Git attempts to divide the large hunk into smaller hunks. Inspect each new patch carefully. Enter `y` for the bug fix and `n` for the debug statement. Splitting depends on the actual lines and context; Git may not be able to separate adjacent edits. If it cannot split, manual editing is the next option.

### Edit a patch manually

At a hunk prompt, enter `e`. Git opens a temporary patch in your configured editor. This patch is a small diff language, not the source file itself.

For each changed source line, the first character controls what the patch means:

- `+` means the line is added in the new version.
- `-` means the line is removed from the old version.
- A leading space means the line is unchanged context.

To leave an added line out, delete that `+` line from the temporary patch. To leave a deletion out, change its leading `-` to a space so the line becomes context (it remains in the file). Keep the `@@ ... @@` hunk header and surrounding context intact. Git can recalculate hunk line counts for an edited patch; if it reports that the patch does not apply, cancel the edit and use `s` or stage the whole hunk instead.

Example original patch:

```diff
@@ -20,3 +20,5 @@
 const total = sum(items);
+console.log(total);
+return total + tax;
 }
```

To stage only the return line and leave the debug print unstaged, remove the `+console.log(total);` line from the temporary patch:

```diff
@@ -20,3 +20,4 @@
 const total = sum(items);
+return total + tax;
 }
```

The exact editor behavior depends on your Git configuration. Save and close the editor to ask Git to apply the edited patch. If Git rejects the patch, the index is not updated by that failed edit; reopen `git add -p`, choose a smaller hunk or make a valid edit, then inspect the result. If you do not want the edit, exit the editor without saving and verify with `git status` and `git diff --staged`.

Manual patch editing is powerful but easy to get wrong. When nearby edits can be staged together safely, prefer `s` followed by `y` or `n`; use `e` when you need finer control.

### Verify the index and working tree separately

After the interactive session, always inspect both sides:

```bash
git diff --staged   # The exact changes the next commit will contain
git diff            # Remaining unstaged changes in tracked files
git status          # Staged, unstaged, and untracked paths
```

Think of `git diff --staged` as **index versus last commit**, and plain `git diff` as **working tree versus index**. A line can appear in one diff but not the other because the index and working tree are separate snapshots.

Before committing, check that the staged diff contains the complete intended change: a staged addition should not depend on an unstaged edit that is required to make it work. Also check that secrets, generated files, and unrelated edits are absent.

### Unstage a hunk without losing the edit

If the wrong hunk entered the index, interactively remove it from the staged snapshot:

```bash
git restore --staged -p -- src/example.js
```

This presents staged changes and lets you select hunks to unstage. It leaves the file contents in the working tree. `git restore --staged src/example.js` unstages all changes for that file, which is simpler when you want to start over. Then rerun `git add -p` and review the staged diff again.

### Commit only the selected pieces

```bash
git diff --staged
git commit -m "Fix total calculation"
git status
git diff
```

The commit includes the index snapshot at commit time. Any unstaged edit remains in the working tree. After committing, it is normal for `git status` to show a modified file: that means part of the file is still waiting for a later commit.

If you staged a new untracked file partially, use `git add -N path/to/file` first to make Git show its contents as a diff, then run `git add -p`. Review the result carefully; new-file partial staging can be less intuitive than staging edits to an already tracked file.

### Common hunk-staging mistakes

- **Thinking `n` discards a change:** it only leaves that hunk unstaged. The edit remains on disk.
- **Thinking `q` rolls back earlier choices:** it stops prompting, but already selected hunks remain staged.
- **Editing source text instead of the patch markers:** the `e` editor edits a patch. Preserve line content and alter the leading marker to change staging selection.
- **Committing without reviewing the index:** `git commit` records staged changes, which may be only part of the file.
- **Assuming stage state is all-or-nothing per file:** the index can represent some lines from a file while other lines remain unstaged.
- **Using `git add .` after carefully staging hunks:** that can stage the rest of the changes too. Recheck `git diff --staged` before committing.

## ➕ More useful commands for everyday work

### Compare versions and inspect commits

```bash
git diff --check                       # Find whitespace errors in unstaged changes
git diff --staged --check              # Check staged changes for whitespace errors
git diff --stat                        # Summarize unstaged changes by file
git diff --staged --stat               # Summarize staged changes by file
git show --stat --oneline <commit>     # Show a commit summary
git log -5 --oneline                   # Show the last five commits
git log --oneline main..feature        # Commits in feature that are not in main
git log --oneline main...feature       # Commits reachable from either side, excluding common history
git diff main...feature                # Changes feature introduces since it split from main
```

`A..B` generally asks which commits are reachable from `B` but not `A`. `A...B` in a commit log shows commits unique to either side. In `git diff`, `main...feature` compares the merge base of `main` and `feature` to `feature`, which is useful for reviewing a branch's proposed changes.

### Apply one commit with cherry-pick

Cherry-pick copies the change from an existing commit onto the current branch and creates a new commit with a new ID. Use it when you need one fix from another branch without merging all of that branch.

```bash
git switch release
git cherry-pick <commit>
```

For several adjacent commits, use a range carefully:

```bash
git cherry-pick <oldest-commit>^..<newest-commit>
```

If conflicts occur, resolve files, stage them, and run `git cherry-pick --continue`. Use `git cherry-pick --abort` to cancel. Cherry-picking duplicates a change in history; when you need the whole branch, merging or rebasing is usually clearer.

### Clean untracked files safely

`git clean` removes files that Git does not track. It does not move them to the Git history, so preview first:

```bash
git clean -n       # Preview untracked files in this directory
git clean -nd      # Preview untracked files and directories
git clean -i       # Review an interactive cleanup menu
```

Only after confirming the preview should you use `git clean -f` or `git clean -fd` to delete files. `-x` also includes ignored files, which can remove build outputs or local configuration; avoid it unless you know exactly what will be removed. `git clean` cannot delete tracked modifications; use `git restore` for those, with the same caution.

### Move and remove tracked files

```bash
git mv old-name.txt new-name.txt   # Move/rename and stage the operation
git rm old-name.txt                # Remove from disk and stage the removal
git rm --cached .env               # Keep the local file, stop tracking it
```

Git detects renames by comparing file contents when it displays history; it does not store a separate rename object. Use `git status` and `git diff --staged` to confirm the move or removal is staged as intended.

### Inspect repository and configuration details

```bash
git remote -v                  # Show remote names and fetch/push URLs
git remote show origin         # Show remote branches and tracking setup
git branch -vv                 # Show upstream and ahead/behind counts
git config --show-origin --list # Show settings and which file supplied them
git rev-parse --show-toplevel  # Print the repository root
git rev-parse HEAD             # Print the current commit ID
git describe --tags --always   # Name the current commit using a nearby tag when possible
```

Configuration can be set at system, global, or repository scope. Repository-local values override global values, which override system values. `git config --show-origin --list` helps explain why a setting has its current value. Be careful before sharing its output because configuration may include private remote URLs or user details.

### Submodules: repositories inside a repository

A submodule records a specific commit of another repository at a path. It is useful when a project intentionally depends on another Git repository, but it adds setup and update steps.

```bash
git submodule add https://github.com/OWNER/DEPENDENCY.git vendor/dependency
git status
git commit -m "Add dependency submodule"
```

A fresh clone can initialize the recorded submodules with:

```bash
git clone --recurse-submodules <repository-url>
```

If already cloned:

```bash
git submodule update --init --recursive
```

After changing a submodule to a different commit, the parent repository records the new pointer. Review and commit that pointer change in the parent repo. A submodule is not automatically updated just because its upstream repository changed; choose the desired dependency commit explicitly.

## 🛟 Repair commits without losing work

Before changing commit history, inspect the current state:

```bash
git status
git log --oneline -5
git diff
git diff --staged
```

### Amend the latest local commit

Use `--amend` when the last commit is yours and has not been shared, for example to correct its message or include a forgotten file:

```bash
git add forgotten-file.js
git commit --amend --no-edit      # Keep the old message
# or: git commit --amend -m "Correct commit message"
```

Amending replaces the commit with a new commit ID. If the old commit was pushed, other clones may still reference it. Prefer making a follow-up commit for shared history; amend only when you understand the coordination and any required force push.

### Reset moves the branch; restore changes files

`git reset` moves the current branch pointer and optionally updates the index and working tree. The default mode is `--mixed`.

| Command | Branch pointer | Index | Working tree |
| --- | --- | --- | --- |
| `git reset --soft HEAD~1` | Moves back one commit | Keeps changes staged | Keeps changes |
| `git reset --mixed HEAD~1` | Moves back one commit | Unstages changes | Keeps changes |
| `git reset --hard HEAD~1` | Moves back one commit | Resets it | Resets files; local edits can be lost |

For example, to undo the most recent unpushed commit but keep its changes available to recommit:

```bash
git reset --soft HEAD~1
git status
git diff --staged
```

Avoid `--hard` unless you have checked the target and are certain you do not need the affected work. `git reset --hard` can discard staged and unstaged changes. For a commit already shared with others, `git revert <commit>` is generally safer because it adds an undo commit instead of rewriting the branch.

### Restore a file from a specific version

```bash
git restore --source=HEAD -- path/to/file   # Replace from current commit
git restore --source=<commit> -- path/to/file # Replace from another commit
git restore --source=HEAD --staged --worktree -- path/to/file # Reset index and working copy
```

These commands overwrite the selected file state. Inspect or save local edits first. `git restore --source=<commit> -- path` changes the working tree by default; add `--staged` when you also want to replace the index version.

### Recover a commit using reflog

The reflog records recent local movements of `HEAD` and branch references. It is often useful after a mistaken reset, amend, or rebase.

```bash
git reflog --date=local
```

Find the entry representing the commit or branch position you want. Before moving anything, create a safety branch pointing at that commit:

```bash
git branch rescue/before-recovery <commit-id>
```

You can inspect it using `git show <commit-id>` or `git switch --detach <commit-id>`. If you have confirmed that you want the current branch to point there, switch back to the branch and reset it deliberately. Reflog entries are local and expire over time; they are a recovery aid, not a permanent backup.

## 🌐 Safer remote collaboration

A remote is a named URL. `origin` is only a conventional name; a repository can have several remotes. Fetch first when you want to inspect incoming work without changing your current branch.

```bash
git remote -v
git fetch origin
git log --oneline --left-right HEAD...origin/main
git diff HEAD...origin/main
git status -sb
```

After fetch, `origin/main` is your local record of the remote's `main` branch. If your current branch is `main` and you want to accept only a fast-forward update:

```bash
git switch main
git pull --ff-only origin main
```

`git pull` performs a fetch and then integrates the selected remote branch. Depending on config, integration may use a fast-forward, merge, or rebase. If you want to make that choice explicit, fetch and then run `git merge origin/main` or `git rebase origin/main` yourself. Do not pull blindly when you have uncommitted edits; commit, stash, or otherwise account for them first.

If a push is rejected because the remote has commits you do not have, fetch and inspect the divergence. Integrate the remote changes, resolve any conflicts, then push again. Avoid force push as a routine fix. If rewriting your own branch is agreed and necessary, `git push --force-with-lease` checks that the remote ref has not advanced unexpectedly; it can still replace remote history, so coordinate before using it on shared branches.

## 🌳 Worktrees in more detail

A worktree is another checkout linked to the same repository database. It shares commits, refs, and objects, while each directory has its own working files and index. This is handy when you need to inspect or fix another branch without disturbing unfinished work in your current checkout.

```bash
git worktree list
git worktree add ../urgent-fix -b fix/urgent origin/main
# Work in ../urgent-fix, make and commit the fix
git worktree remove ../urgent-fix
git worktree prune
```

The branch given to `git worktree add` cannot already be checked out in another worktree. `git worktree remove` expects a clean worktree by default; inspect and preserve its changes before removing it. `git worktree prune` cleans stale records for worktree directories that were deleted outside Git. It does not delete a normal branch or its commits.

## 🔍 Bisect with a repeatable test

Bisect narrows down which commit first introduced a regression by checking revisions between a known-good and known-bad point. At each step, run the same test and report whether that revision is good or bad.

```bash
git bisect start
git bisect bad HEAD
git bisect good v1.2.0
# Run the regression test on the checked-out revision
git bisect good  # use this if the test passes
git bisect bad   # use this if the test fails
# Repeat until Git identifies the first bad commit
git bisect reset
```

For a reliable result, use a test that gives the same answer each time and make sure it works on older revisions. If a revision cannot be tested because it does not build, mark it `git bisect skip`; Git will search around it and may identify a range rather than one exact commit. Always finish with `git bisect reset` to return to your original checkout.

If the test is a command that exits zero for good and nonzero for bad, automate the checks:

```bash
git bisect start
git bisect bad HEAD
git bisect good <known-good-commit>
git bisect run ./scripts/check-regression.sh
git bisect reset
```

## 📦 Submodule lifecycle and common gotchas

A parent repository stores a submodule's path, its source URL in `.gitmodules`, and the exact commit checked out there. It does not store the dependency's files as ordinary parent-repository files. Cloning the parent alone may therefore leave the submodule directory uninitialized.

```bash
git submodule status
git submodule update --init --recursive
git submodule update --remote vendor/dependency # Move to the configured upstream tip
```

The last command changes the checked-out submodule commit to a newer upstream commit; review and commit the resulting pointer change in the parent repository. It does not automatically mean “use the newest stable release.” For reproducible builds, commit a specific reviewed dependency pointer. When changing a submodule URL, edit `.gitmodules`, then synchronize local Git config:

```bash
git submodule sync --recursive
```

Before committing the parent, check both the submodule's own status and the parent status. A modified submodule may contain uncommitted work inside its own repository; that work must be committed or otherwise preserved there before the parent can record a useful pointer.

## 🧭 A compact command priority map

| Learn first | Learn next | Learn when needed |
| --- | --- | --- |
| `status`, `diff`, `add`, `commit` | `fetch`, `pull`, `push`, `remote` | `rebase -i`, `cherry-pick` |
| `log`, `show`, `switch`, `branch` | `merge`, conflict resolution | `bisect`, `worktree`, submodules |
| `restore --staged`, `revert` | `stash`, `tag`, `.gitignore` | `reflog`, careful `reset`, `clean` |

The commands in the last column can rewrite history, remove files, or add repository complexity. Learn their effects first and inspect `git status` before and after using them.
