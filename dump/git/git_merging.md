# GIT MERGING AND CONFLICTS

## TYPES OF MERGES

### FAST-FORWARD MERGE

When the target branch hasn't diverged from the source branch:

```bash
git checkout main
git merge feature-branch                   # Fast-forward if possible
git merge --no-ff feature-branch           # Force merge commit
```

### THREE-WAY MERGE

When both branches have new commits:

```bash
git checkout main
git merge feature-branch                   # Creates merge commit
```

### SQUASH MERGE

Combines all commits from feature branch into one:

```bash
git checkout main
git merge --squash feature-branch
git commit -m "Add feature: combined all changes"
```

## MERGE STRATEGIES

```bash
# Default recursive strategy
git merge feature-branch

# Ours strategy (keep our version)
git merge -X ours feature-branch

# Theirs strategy (prefer their version)
git merge -X theirs feature-branch

# Octopus merge (multiple branches)
git merge branch1 branch2 branch3
```

## UNDERSTANDING CONFLICTS

Conflicts occur when Git can't automatically merge changes. Common scenarios:

- Same line modified in both branches
- File deleted in one branch, modified in another
- Binary files changed in both branches

### CONFLICT MARKERS

```
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> feature-branch
```

## RESOLVING CONFLICTS

### MANUAL RESOLUTION

```bash
# 1. Start merge
git merge feature-branch

# 2. Check conflict status
git status

# 3. Edit conflicted files manually
# Remove conflict markers and choose/combine changes

# 4. Mark as resolved
git add <resolved-file>

# 5. Complete merge
git commit                                 # Don't use -m, let Git generate message
```

### USING MERGE TOOLS

```bash
# Configure merge tool
git config --global merge.tool vimdiff     # or meld, kdiff3, etc.

# Launch merge tool
git mergetool

# Available tools: vimdiff, meld, kdiff3, opendiff, emerge, tortoisemerge
```

### ABORTING MERGE

```bash
# Cancel merge process
git merge --abort

# Reset to pre-merge state
git reset --hard HEAD
```

## ADVANCED CONFLICT RESOLUTION

### SHOW CONFLICT DETAILS

```bash
# Show both sides of conflict
git diff

# Show common ancestor
git merge-base HEAD feature-branch
git show $(git merge-base HEAD feature-branch)

# Show three-way diff
git diff HEAD...feature-branch
```

### CHECKOUT SPECIFIC VERSION

```bash
# Take our version
git checkout --ours <file>

# Take their version
git checkout --theirs <file>

# From specific commit
git checkout <commit-hash> -- <file>
```

## MERGE COMMIT MESSAGES

```bash
# Default merge message
git merge feature-branch

# Custom merge message
git merge feature-branch -m "Merge feature: add user authentication"

# Edit merge message in editor
git merge feature-branch --edit
```

## PREVENTING CONFLICTS

### GOOD PRACTICES

1. **Keep branches up-to-date**

```bash
git checkout feature-branch
git merge main                             # Or rebase
```

2. **Small, frequent commits**
3. **Clear communication in teams**
4. **Use .gitattributes for line endings**

### GIT ATTRIBUTES FOR MERGE

```bash
# .gitattributes file
*.png binary
*.jpg binary
*.md merge=ours                            # Always use our version
package-lock.json merge=npm                # Custom merge driver
```

## THREE-WAY MERGE VISUALIZATION

```
    A---B---C topic
   /         \
  D---E---F---G main
```

## MERGE VS REBASE DECISION TREE

**Use Merge when:**

- You want to preserve branch history
- Working on public/shared branches
- Feature is complete and tested

**Use Rebase when:**

- You want linear history
- Working on private feature branch
- Want to clean up commits before merging

## OCTOPUS MERGE

Merging multiple branches simultaneously:

```bash
# Merge multiple feature branches
git checkout main
git merge feature1 feature2 feature3

# Only works if no conflicts occur
# If conflicts, fall back to individual merges
```

## MERGE HOOKS

### PRE-MERGE HOOK

```bash
#!/bin/sh
# .git/hooks/pre-merge-commit

# Run tests before creating merge commit
npm test
if [ $? -ne 0 ]; then
    echo "Tests failed. Merge aborted."
    exit 1
fi
```

## TROUBLESHOOTING MERGES

```bash
# Find problematic merge
git log --merges --oneline

# Show merge commit details
git show <merge-commit-hash>

# Undo merge (if not pushed)
git reset --hard HEAD~1

# Undo merge (if pushed) - creates new commit
git revert -m 1 <merge-commit-hash>
```

## MERGE STRATEGIES COMPARISON

| Strategy | Use Case | History | Conflicts |
|----------|----------|---------|-----------|
| Fast-forward | Linear development | Clean | None |
| Merge commit | Feature integration | Preserves branches | Manual resolution |
| Squash | Clean main branch | Single commit | Manual resolution |
| Rebase | Linear history | Rewrites | Per commit |

---
