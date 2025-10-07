# GIT REMOTE REPOSITORIES

## UNDERSTANDING REMOTES

Remotes are versions of your repository hosted on the internet or network. They enable collaboration and backup of your code.

## BASIC REMOTE OPERATIONS

```bash
# Add remote repository
git remote add origin <repository-url>
git remote add upstream <original-repo-url>  # For forks

# List remotes
git remote
git remote -v                               # Show URLs

# Show remote information
git remote show origin

# Remove remote
git remote remove <remote-name>

# Rename remote
git remote rename <old-name> <new-name>
```

## FETCHING AND PULLING

```bash
# Fetch changes (doesn't merge)
git fetch origin
git fetch --all                            # Fetch from all remotes

# Pull changes (fetch + merge)
git pull origin main
git pull                                   # Pull from tracked branch

# Pull with rebase instead of merge
git pull --rebase origin main
git config --global pull.rebase true      # Set as default
```

## PUSHING CHANGES

```bash
# Push to remote
git push origin main
git push                                   # Push to tracked branch

# Push new branch and set upstream
git push -u origin <branch-name>

# Push all branches
git push --all origin

# Push tags
git push --tags
git push origin <tag-name>

# Force push (dangerous!)
git push --force origin main
git push --force-with-lease origin main   # Safer force push
```

## WORKING WITH FORKS

```bash
# Fork workflow
git clone <your-fork-url>
git remote add upstream <original-repo-url>

# Sync with upstream
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

# Create pull request
git checkout -b feature/new-feature
# ... make changes ...
git push -u origin feature/new-feature
# Create PR on GitHub/GitLab
```

## REMOTE BRANCHES

```bash
# List remote branches
git branch -r
git branch -a                              # All branches

# Track remote branch
git checkout --track origin/<branch-name>
git checkout -b <local-name> origin/<remote-branch>

# Delete remote branch
git push origin --delete <branch-name>

# Prune deleted remote branches
git remote prune origin
git fetch --prune
```

## SSH VS HTTPS

### HTTPS

- Easier to set up
- Works through firewalls
- Requires username/password or token

### SSH

- More secure
- No password prompts once set up
- Requires SSH key setup

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Add to SSH agent
ssh-add ~/.ssh/id_ed25519

# Test SSH connection
ssh -T git@github.com
```

## MULTIPLE REMOTES

```bash
# Working with multiple remotes
git remote add origin <your-repo>
git remote add upstream <original-repo>
git remote add backup <backup-repo>

# Push to multiple remotes
git push origin main
git push backup main

# Set up push to multiple remotes
git remote set-url --add --push origin <url1>
git remote set-url --add --push origin <url2>
```

## REMOTE CONFIGURATION

```bash
# Configure default push behavior
git config --global push.default simple

# Configure pull behavior
git config --global pull.rebase false     # Merge (default)
git config --global pull.rebase true      # Rebase
git config --global pull.ff only          # Fast-forward only

# Store credentials (HTTPS)
git config --global credential.helper store
git config --global credential.helper cache
```

## TROUBLESHOOTING REMOTE ISSUES

```bash
# Reset remote URL
git remote set-url origin <new-url>

# Fix diverged branches
git pull --rebase origin main

# Handle rejected push
git pull origin main
git push origin main

# Or force push (be careful!)
git push --force-with-lease origin main
```

---
