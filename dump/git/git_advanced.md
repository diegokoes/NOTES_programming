# GIT ADVANCED OPERATIONS

## REBASING

### BASIC REBASE
```bash
# Rebase current branch onto main
git checkout feature-branch
git rebase main

# Rebase specific branch
git rebase main feature-branch

# Continue after resolving conflicts
git rebase --continue

# Skip current commit
git rebase --skip

# Abort rebase
git rebase --abort
```

### INTERACTIVE REBASE
```bash
# Interactive rebase last 3 commits
git rebase -i HEAD~3

# Interactive rebase from specific commit
git rebase -i <commit-hash>

# Rebase from root (all commits)
git rebase -i --root
```

Interactive rebase commands:
- `pick` (p): Use commit as-is
- `reword` (r): Change commit message
- `edit` (e): Stop to amend commit
- `squash` (s): Combine with previous commit
- `fixup` (f): Like squash but discard commit message
- `drop` (d): Remove commit

### REBASE VS MERGE
```bash
# Before rebase:     After rebase:
#   A---B---C topic     A'--B'--C' topic
#  /                   /
# D---E---F main      D---E---F main

# Before merge:      After merge:
#   A---B---C topic     A---B---C topic
#  /         \         /         \
# D---E---F---G main  D---E---F---G main
```

## CHERRY-PICKING
Apply specific commits from one branch to another:
```bash
# Cherry-pick single commit
git cherry-pick <commit-hash>

# Cherry-pick multiple commits
git cherry-pick <commit1> <commit2>

# Cherry-pick range (exclusive)
git cherry-pick <start-commit>..<end-commit>

# Cherry-pick with different commit message
git cherry-pick <commit-hash> --no-commit
git commit -m "New message"

# Continue after conflict
git cherry-pick --continue

# Abort cherry-pick
git cherry-pick --abort
```

## STASHING
Temporarily save work without committing:
```bash
# Stash current changes
git stash
git stash push -m "Work in progress on feature X"

# Stash including untracked files
git stash -u

# List stashes
git stash list

# Apply stash
git stash apply                            # Keep stash in list
git stash pop                              # Apply and remove from list
git stash apply stash@{2}                  # Apply specific stash

# Drop stash
git stash drop stash@{1}
git stash clear                            # Remove all stashes

# Show stash contents
git stash show
git stash show -p stash@{1}                # Show changes
```

## ADVANCED STASHING
```bash
# Stash only staged changes
git stash --staged

# Stash specific files
git stash push -m "Partial work" -- file1.js file2.js

# Create branch from stash
git stash branch <branch-name> stash@{1}

# Interactive stashing
git stash -p                               # Choose hunks to stash
```

## REFLOG
Git's safety net - tracks all repository changes:
```bash
# Show reflog
git reflog
git reflog --all                           # All references

# Show reflog for specific branch
git reflog <branch-name>

# Recover lost commits
git reflog
git checkout <lost-commit-hash>
git checkout -b recovery-branch

# Reflog with dates
git reflog --since="2 hours ago"
```

## RESET OPERATIONS
```bash
# Soft reset (keep changes staged)
git reset --soft HEAD~1

# Mixed reset (unstage changes, default)
git reset HEAD~1
git reset --mixed HEAD~1

# Hard reset (discard all changes)
git reset --hard HEAD~1

# Reset to specific commit
git reset --hard <commit-hash>

# Reset specific file
git reset HEAD <file>                      # Unstage file
git checkout HEAD -- <file>               # Discard changes
```

## GIT BISECT
Binary search to find problematic commits:
```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark known good commit
git bisect good <commit-hash>

# Git will checkout middle commit
# Test and mark as good or bad
git bisect good                            # or git bisect bad

# Continue until found
# Reset when done
git bisect reset

# Automate with script
git bisect run ./test-script.sh
```

## WORKTREES
Multiple working directories for same repository:
```bash
# Create new worktree
git worktree add ../project-feature feature-branch

# List worktrees
git worktree list

# Remove worktree
git worktree remove ../project-feature

# Prune deleted worktrees
git worktree prune
```

## SUBMODULES
Include other Git repositories as subdirectories:
```bash
# Add submodule
git submodule add <repository-url> <path>

# Initialize submodules
git submodule init
git submodule update

# Clone with submodules
git clone --recursive <repository-url>

# Update submodules
git submodule update --remote

# Remove submodule
git submodule deinit <submodule-path>
git rm <submodule-path>
```

## GIT HOOKS
Automate tasks at Git events:

### CLIENT-SIDE HOOKS
- `pre-commit`: Before commit creation
- `prepare-commit-msg`: Before commit message editor
- `commit-msg`: Validate commit message
- `post-commit`: After commit creation
- `pre-rebase`: Before rebase
- `post-checkout`: After checkout
- `post-merge`: After merge

### SERVER-SIDE HOOKS
- `pre-receive`: Before push acceptance
- `update`: Before branch update
- `post-receive`: After push completion

### EXAMPLE PRE-COMMIT HOOK
```bash
#!/bin/sh
# .git/hooks/pre-commit

# Run linting
npm run lint
if [ $? -ne 0 ]; then
    echo "Linting failed. Please fix and try again."
    exit 1
fi

# Run tests
npm test
if [ $? -ne 0 ]; then
    echo "Tests failed. Please fix and try again."
    exit 1
fi
```

## ADVANCED LOG AND DIFF
```bash
# Log with custom format
git log --pretty=format:"%C(yellow)%h %C(red)%d %C(reset)%s %C(green)[%cn]" --graph

# Show changes between commits
git diff HEAD~2..HEAD~1

# Show changes in specific file
git log -p -- <file-path>

# Show commits that touch specific line
git log -L <start>,<end>:<file-path>

# Find when line was added/removed
git log -S "function_name" -- <file-path>

# Show merge commits only
git log --merges

# Show no-merge commits only
git log --no-merges
```

## GIT ATTRIBUTES
Configure Git behavior per file type:
```bash
# .gitattributes
*.txt text
*.jpg binary
*.sh text eol=lf
*.bat text eol=crlf
secrets.txt filter=secret

# Custom diff driver
*.json diff=json

# Custom merge driver
*.generated merge=ours
```

## PERFORMANCE OPTIMIZATION
```bash
# Garbage collection
git gc

# Aggressive garbage collection
git gc --aggressive

# Prune unreachable objects
git prune

# Check repository size
git count-objects -vH

# Reduce repository size
git repack -ad

# Remove large files from history
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch large-file.zip' --prune-empty --tag-name-filter cat -- --all
```

---

