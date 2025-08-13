# GIT BRANCHING

## BRANCH FUNDAMENTALS
Branches in Git are lightweight movable pointers to specific commits. They allow you to work on different features or experiments without affecting the main codebase.

## BASIC BRANCH OPERATIONS
```bash
# List branches
git branch                  # Local branches only
git branch -a              # All branches (local and remote)
git branch -r              # Remote branches only

# Create new branch
git branch <branch-name>
git checkout -b <branch-name>     # Create and switch in one command
git switch -c <branch-name>       # Modern alternative to checkout -b

# Switch between branches
git checkout <branch-name>
git switch <branch-name>          # Modern alternative to checkout

# Delete branches
git branch -d <branch-name>       # Safe delete (only if merged)
git branch -D <branch-name>       # Force delete
git push origin --delete <branch-name>  # Delete remote branch
```

## BRANCH INFORMATION
```bash
# Show current branch
git branch --show-current

# Show branch relationships
git show-branch
git log --graph --oneline --all

# Show branches that contain a specific commit
git branch --contains <commit-hash>

# Show last commit on each branch
git branch -v
```

## BRANCH STRATEGIES

### GIT FLOW
- **main/master**: Production-ready code
- **develop**: Integration branch for features
- **feature/**: Individual feature development
- **release/**: Prepare new releases
- **hotfix/**: Critical fixes for production

### GITHUB FLOW
- **main**: Always deployable
- **feature branches**: Short-lived branches for specific features
- Pull requests for code review before merging

### GITLAB FLOW
- Similar to GitHub Flow but with environment branches
- **main** → **pre-production** → **production**

## ADVANCED BRANCH OPERATIONS
```bash
# Rename branch
git branch -m <old-name> <new-name>
git branch -m <new-name>          # Rename current branch

# Set upstream branch
git push -u origin <branch-name>
git branch --set-upstream-to=origin/<branch-name>

# Track remote branch
git checkout --track origin/<branch-name>

# Create branch from specific commit
git checkout -b <branch-name> <commit-hash>

# Compare branches
git diff main..feature-branch
git log main..feature-branch
```

## BRANCH PROTECTION
Best practices for protecting important branches:
- Require pull request reviews
- Require status checks to pass
- Require branches to be up to date before merging
- Restrict pushes to certain users/teams
- Require signed commits

## COMMON BRANCHING PATTERNS
```bash
# Feature branch workflow
git checkout main
git pull origin main
git checkout -b feature/new-feature
# ... make changes ...
git push -u origin feature/new-feature
# Create pull request, review, merge

# Hotfix workflow
git checkout main
git checkout -b hotfix/critical-fix
# ... make fix ...
git checkout main
git merge hotfix/critical-fix
git tag v1.0.1
git push origin main --tags
```

---
#git
