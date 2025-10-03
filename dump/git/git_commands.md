# Git Commands — Quick Reference

## Table of contents

- [Purpose & conventions](#purpose--conventions)
- [Porcelain vs plumbing](#porcelain-vs-plumbing)
- [Common workflows (quick links)](#common-workflows-quick-links)
- [Commands by category](#commands-by-category)
  - [Setup / config](#setup--config)
  - [Repository info & status](#repository-info--status)
  - [Staging & commits](#staging--commits)
  - [Branching](#branching)
  - [Merging & rebase](#merging--rebase)
  - [Syncing remotes](#syncing-remotes)
  - [Undoing changes](#undoing-changes)
  - [History & inspection](#history--inspection)
  - [Stash, tags, submodules](#stash-tags-submodules)
  - [Plumbing (for scripts/tools)](#plumbing-for-scripts-tools)
- [Examples & script-friendly outputs](#examples--script-friendly-outputs)
- [Aliases & tips](#aliases--tips)
- [References](#references)

---

## Purpose & conventions

This file is a compact, script-friendly reference of commonly used Git commands. Each entry follows this small contract:

- Syntax: inline code (or fenced example)
- Short description
- Example (fenced block) when helpful
- Notes (edge cases, script-parsing guidance)

Use `git <command> --help` for full details.

> [!tip] Parser-friendly
> Prefer `git status --porcelain` (or porcelain-style commands) for scripts and prompts. Plumbing commands are lower-level and useful in tooling.

---

## Porcelain vs plumbing

- Porcelain: user-oriented commands and stable flags meant for humans and many scripts. Example: `git status --porcelain`.
- Plumbing: low-level commands used by tools and libraries. Example: `git ls-files`, `git cat-file`.

Use porcelain when you want stable, parsable output for CI, prompt detection, or dotfiles. Use plumbing when you need raw object access.

---

## Common workflows (quick links)

- Initialize / clone → configure → create branch → commit → push
- Inspect / fix mistakes → `git log`, `git restore`, `git revert`, `git rebase -i`
- Sync with remote → `git fetch`, `git pull --rebase`, `git push`

---

## Commands by category

### Setup / config

- `git config --global user.name "Your Name"`
  - Set your identity.

- `git init [dir]`
  - Create a new repository.

- `git clone <url>`
  - Clone a remote repository.

- `git remote add origin <url>`
  - Add a remote named `origin`.

### Repository info & status

- `git status`
  - Human-readable working tree summary.

- `git status --porcelain -b`
  - Syntax: `git status --porcelain -b`
  - Description: Compact, stable output intended for scripts. The `-b` adds branch and upstream info.
  - Example output:

  ```
  ## main...origin/main
  M src/app.py
  A  newfile.txt
  ?? untracked.file
  ```

  - Notes:
  - Lines starting with `##` contain branch and upstream info.
  - Non-branch lines use two columns (staged, unstaged). A leading `?` indicates untracked files.
  - Empty output = clean working directory (useful in scripts).

- `git remote -v`
  - Show remotes and their URLs.

### Staging & commits

- `git add <pathspec>`
  - Stage changes.

- `git restore --staged <file>`
  - Unstage a file (preferred to `git reset` for single files).

- `git commit -m "msg"`
  - Create a commit.

- `git commit --amend`
  - Amend the last commit (don't amend pushed commits unless you know what you're doing).

### Branching

- `git branch`
  - List local branches.

- `git branch <name>`
  - Create a new branch.

- `git switch <branch>`
  - Switch branches (modern command).

- `git switch -c <branch>` or `git checkout -b <branch>`
  - Create and switch.

- `git branch -d <branch>` / `-D` to force delete.

### Merging & rebase

- `git merge <branch>`
  - Merge another branch into the current branch.

- `git rebase <branch>`
  - Reapply commits on top of another branch.

- `git rebase -i <base>`
  - Interactive rebase for rewriting history.

### Syncing remotes

- `git fetch [remote] [refspec]`
  - Update remote refs without merging.

- `git pull --rebase`
  - Fetch and rebase local commits on top of remote.

- `git push origin <branch>`
  - Push branch to remote.

- Convert SSH remote to HTTPS (safe one-liner):

 ```bash
 ssh_url=$(git remote get-url origin)
 https_url=$(echo "$ssh_url" | sed -E 's|git@([^:]+):(.+)|https://\1/\2|')
 git remote set-url origin "$https_url"
 git remote -v
 ```

 > [!warning] Credentials
 > You may be prompted for credentials when switching to HTTPS unless a credential helper is configured.

### Undoing changes

- `git restore <file>`
  - Restore working-tree file from `HEAD` (or a specified commit).

- `git reset [--soft|--mixed|--hard] <commit>`
  - Move `HEAD`. `--soft` keeps index, `--mixed` (default) updates index, `--hard` resets working tree.

- `git revert <commit>`
  - Create a new commit that undoes `commit` (safe for published history).

### History & inspection

- `git log --oneline --graph --decorate --all`
  - Compact, visual history.

- `git show <commit>`
  - Show commit details and patch.

- `git diff [<commit>] [--] [<path>]`
  - Show differences.

- `git blame <file>`
  - Annotate file with last-change information per line.

### Stash, tags, submodules

- `git stash push -m "msg"`
  - Stash local changes.

- `git stash pop`
  - Apply and drop stash.

- `git tag -a v1.0 -m "release"`
  - Create an annotated tag.

- `git submodule update --init --recursive`
  - Initialize/update submodules.

### Plumbing (for scripts/tools)

- `git rev-parse --abbrev-ref HEAD`
  - Get current branch name (useful in prompts and scripts).

- `git ls-files -m`
  - List modified files.

- `git rev-list --count HEAD`
  - Count commits.

---

## Examples & script-friendly outputs

- Check for a clean working tree (script-friendly):

 ```bash
 if [ -z "$(git status --porcelain)" ]; then
  echo "clean"
 else
  echo "dirty"
 fi
 ```

- Get branch name in a script:

 ```bash
 branch=$(git rev-parse --abbrev-ref HEAD)
 ```

- One-liner to change origin from SSH to HTTPS (idempotent-ish):

 ```bash
 git remote get-url origin | sed -E 's|git@([^:]+):(.+)|https://\1/\2|' | xargs -I{} git remote set-url origin {}
 ```

---

## Aliases & tips

- Useful aliases (set with `git config --global`):

 ```bash
 git config --global alias.co checkout
 git config --global alias.st "status"
 git config --global alias.ll "log --oneline --graph --decorate"
 ```

- Tips:
  - Prefer `git switch` / `git restore` on modern Git for clearer intent.
  - Use porcelain (`--porcelain`) or plumbing consistently in scripts.
  - Add a small TOC or headings to long notes so Obsidian/GitHub can link directly to sections.

## References

- `git help <command>`
- Pro Git book: <https://git-scm.com/book/en/v2>
- Git docs: <https://git-scm.com/docs>
