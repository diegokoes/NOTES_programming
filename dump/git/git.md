# git
## Summary
> [!summary]
> Git is a distributed version control system that tracks changes in files and coordinates work among multiple developers. Created by Linus Torvalds in 2005, Git allows developers to maintain a complete history of their project, work on different features simultaneously through branches, and collaborate efficiently with others. Unlike centralized version control systems, every Git repository contains the full project history, making it resilient and enabling offline work.

## Theory
Git operates on a simple but powerful model with three main areas:
- **Working Directory**: Your current files
- **Staging Area**: Files prepared for commit
- **Repository**: Complete project history

### Core Concepts
- [[git_basics|Basics]] - Git initialization, configuration, and fundamental workflow
- [[git_branching|Branching]] - Creating, managing, and merging branches
- [[git_remote|Remote Repositories]] - Working with remote repositories (GitHub, GitLab, etc.)
- [[git_commits|Commits]] - Understanding commits, commit messages, and history
- [[git_merging|Merging & Conflicts]] - Merge strategies, conflicts, and resolution
- [[git_advanced|Advanced Operations]] - Rebasing, cherry-picking, hooks, and advanced workflows
- [[git_troubleshooting|Troubleshooting]] - Common problems and how to fix them

## Questions
> [!tip]- Easy: What are the three main areas in Git's architecture?
> The three main areas are:
> 1. **Working Directory** - where you edit files
> 2. **Staging Area** (Index) - where you prepare files for commit
> 3. **Repository** - where Git stores the complete project history

> [!tip]- Easy: What command initializes a new Git repository?
> `git init` - This creates a new .git directory in your current folder, turning it into a Git repository.

> [!tip]- Easy: How do you check the status of your working directory?
> `git status` - Shows which files are modified, staged, or untracked.

> [!warning]- Medium: Explain the difference between `git merge` and `git rebase`.
> - **git merge**: Creates a new commit that combines changes from two branches, preserving the branch history as a merge commit.
> - **git rebase**: Replays commits from one branch onto another, creating a linear history without merge commits. It "rewrites" history by changing commit hashes.

> [!warning]- Medium: What's the difference between `git reset --soft`, `git reset --mixed`, and `git reset --hard`?
> - `--soft`: Moves HEAD but keeps staging area and working directory unchanged
> - `--mixed` (default): Moves HEAD and resets staging area, but keeps working directory unchanged
> - `--hard`: Moves HEAD and resets both staging area and working directory (destructive)

> [!warning]- Medium: How would you undo the last commit but keep the changes in your working directory?
> `git reset --soft HEAD~1` - This moves the branch pointer back one commit but keeps all changes staged and ready to be committed again.

> [!danger]- Hard: Explain the Git object model and the relationship between blobs, trees, and commits.
> Git stores data as objects:
> - **Blob**: Stores file content (identified by SHA-1 hash of content)
> - **Tree**: Stores directory structure, referencing blobs and other trees
> - **Commit**: Points to a tree (project snapshot) and contains metadata (author, timestamp, message, parent commits)
> - **Tag**: Points to a commit with additional metadata
> 
> Each commit points to a tree representing the project state at that moment, and commits form a directed acyclic graph through parent relationships.

> [!danger]- Hard: How would you recover a commit that was accidentally deleted with `git reset --hard`?
> Use `git reflog` to find the commit hash, then:
> 1. `git reflog` - Shows recent HEAD movements
> 2. Find the commit hash before the reset
> 3. `git reset --hard <commit-hash>` or `git checkout -b recovery-branch <commit-hash>`
> 
> Git keeps unreachable commits for ~30 days before garbage collection.

> [!danger]- Hard: Explain interactive rebase and when you would use it.
> `git rebase -i <base-commit>` allows you to modify commit history by:
> - **pick**: Use commit as-is
> - **reword**: Change commit message
> - **edit**: Stop to modify the commit
> - **squash**: Combine with previous commit
> - **drop**: Remove the commit
> 
> Use cases: Clean up commit history before merging, combine related commits, fix commit messages, or reorder commits. Never rebase commits that have been pushed to shared repositories.






- - -
#git