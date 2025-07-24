# Git Troubleshooting

## Common Problems and Solutions

### "Detached HEAD" State
When you checkout a specific commit instead of a branch:
```bash
# Check current state
git status

# Create new branch from current position
git checkout -b new-branch-name

# Or return to a branch
git checkout main
```

### Accidentally Committed to Wrong Branch
```bash
# Move last commit to new branch
git branch new-branch
git reset --hard HEAD~1
git checkout new-branch

# Move last commit to existing branch
git checkout existing-branch
git cherry-pick main
git checkout main
git reset --hard HEAD~1
```

### Undo Last Commit
```bash
# Keep changes but undo commit
git reset --soft HEAD~1

# Undo commit and unstage changes
git reset HEAD~1

# Completely remove commit and changes
git reset --hard HEAD~1

# Undo commit but create new commit (safe for shared repos)
git revert HEAD
```

### Recovering Lost Commits
```bash
# Find lost commits
git reflog

# Checkout lost commit
git checkout <commit-hash>

# Create branch from lost commit
git checkout -b recovery-branch <commit-hash>

# Or reset branch to lost commit
git reset --hard <commit-hash>
```

### Force Push Gone Wrong
```bash
# Find previous state
git reflog

# Reset to previous state
git reset --hard <previous-commit>

# Force push to restore
git push --force-with-lease origin main
```

### Merge Conflicts
```bash
# Check conflict status
git status

# See conflicted files
git diff --name-only --diff-filter=U

# Abort merge
git merge --abort

# After resolving conflicts
git add <resolved-files>
git commit
```

### Rebase Conflicts
```bash
# During rebase, after resolving conflicts
git add <resolved-files>
git rebase --continue

# Skip problematic commit
git rebase --skip

# Abort rebase
git rebase --abort
```

### Working Directory Issues

#### Discard All Local Changes
```bash
# Discard unstaged changes
git checkout -- .

# Remove untracked files
git clean -f

# Remove untracked files and directories
git clean -fd

# Preview what will be deleted
git clean -n
```

#### Restore Deleted Files
```bash
# Restore single file
git checkout HEAD -- <file-path>

# Restore file from specific commit
git checkout <commit-hash> -- <file-path>

# Restore all files
git checkout HEAD -- .
```

### Authentication Issues

#### HTTPS Authentication
```bash
# Clear stored credentials
git config --global --unset credential.helper

# Update remote URL
git remote set-url origin https://token@github.com/user/repo.git

# Use personal access token
git config --global credential.helper store
```

#### SSH Issues
```bash
# Test SSH connection
ssh -T git@github.com

# Add SSH key to agent
ssh-add ~/.ssh/id_rsa

# Check SSH agent
ssh-add -l

# Generate new SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"
```

### Remote Repository Issues

#### Diverged Branches
```bash
# When local and remote have diverged
git fetch origin
git merge origin/main

# Or rebase instead
git fetch origin
git rebase origin/main

# Force update (dangerous!)
git reset --hard origin/main
```

#### Rejected Push
```bash
# Pull first, then push
git pull origin main
git push origin main

# Or pull with rebase
git pull --rebase origin main
git push origin main
```

#### Wrong Remote URL
```bash
# Check current remote
git remote -v

# Update remote URL
git remote set-url origin <new-url>

# Add additional remote
git remote add upstream <url>
```

### Large File Issues

#### File Too Large Error
```bash
# Remove large file from last commit
git reset --soft HEAD~1
git reset HEAD <large-file>
git commit

# Remove from all history (nuclear option)
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch <large-file>' --prune-empty --tag-name-filter cat -- --all
```

#### Use Git LFS for Large Files
```bash
# Install Git LFS
git lfs install

# Track large files
git lfs track "*.psd"
git lfs track "*.zip"

# Add .gitattributes
git add .gitattributes
git commit -m "Add LFS tracking"
```

### Performance Issues

#### Slow Git Operations
```bash
# Garbage collection
git gc

# Repack repository
git repack -ad

# Check repository size
git count-objects -vH

# Enable parallel index preload
git config core.preloadindex true

# Use untracked cache
git config core.untrackedCache true
```

### Line Ending Issues
```bash
# Configure line endings
git config --global core.autocrlf true    # Windows
git config --global core.autocrlf input   # macOS/Linux
git config --global core.autocrlf false   # Manual control

# Fix existing files
git add --renormalize .
git commit -m "Normalize line endings"
```

### Debugging Git Issues

#### Verbose Output
```bash
# Enable debug output
GIT_TRACE=1 git status
GIT_TRACE_SETUP=1 git status
GIT_CURL_VERBOSE=1 git push

# Trace specific operations
GIT_TRACE_PACK_ACCESS=1 git status
GIT_TRACE_PACKET=1 git fetch
```

#### Check Git Configuration
```bash
# Show all configuration
git config --list

# Show configuration sources
git config --list --show-origin

# Check specific setting
git config core.autocrlf
```

### Emergency Recovery

#### Corrupted Repository
```bash
# Check repository integrity
git fsck

# Aggressive integrity check
git fsck --full --strict

# Recover from backup
git clone <backup-url> recovery
cd recovery
git remote add broken /path/to/broken/repo
git fetch broken
```

#### Complete Reset (Nuclear Option)
```bash
# Backup first!
cp -r .git .git.backup

# Re-clone repository
cd ..
git clone <remote-url> project-recovery
cd project-recovery

# Copy over any uncommitted work manually
```

### Prevention Strategies

1. **Regular backups**: Push frequently to remote repositories
2. **Use branches**: Don't work directly on main/master
3. **Small commits**: Easier to debug and revert
4. **Meaningful commit messages**: Help future debugging
5. **Test before pushing**: Use pre-push hooks
6. **Keep repositories clean**: Regular garbage collection

### Useful Aliases for Troubleshooting
```bash
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
git config --global alias.undo 'reset --soft HEAD~1'
git config --global alias.amend 'commit --amend --no-edit'
git config --global alias.tree 'log --graph --pretty=format:"%h %s" --all'
```

---
#git
