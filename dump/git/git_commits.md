# GIT COMMITS

## UNDERSTANDING COMMITS
A commit in Git is a snapshot of your repository at a specific point in time. Each commit contains:
- A unique SHA-1 hash identifier
- Author information (name, email, timestamp)
- Commit message
- Pointer to the project tree (files and directories)
- Pointer(s) to parent commit(s)

## CREATING COMMITS
```bash
# Basic commit
git commit -m "Add new feature"

# Multi-line commit message
git commit -m "Add user authentication" -m "- Implement login functionality" -m "- Add password validation"

# Commit with editor (for longer messages)
git commit

# Add and commit in one step (tracked files only)
git commit -am "Fix bug in user registration"

# Empty commit (useful for triggering CI/CD)
git commit --allow-empty -m "Trigger deployment"
```

## COMMIT MESSAGE BEST PRACTICES
### CONVENTIONAL COMMITS FORMAT
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### COMMON TYPES
- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code style changes (formatting, semicolons, etc.)
- **refactor**: Code refactoring
- **test**: Adding or updating tests
- **chore**: Maintenance tasks

### EXAMPLES
```
feat(auth): add OAuth2 login support

Implemented Google and GitHub OAuth2 providers.
Added user profile management and session handling.

Closes #123
```

```
fix: resolve memory leak in image processing

The image processing function was not properly releasing
memory after processing large files.

fixes #456
```

## VIEWING COMMIT HISTORY
```bash
# Basic log
git log
git log --oneline
git log --graph --oneline --all

# Limit number of commits
git log -n 5
git log --since="2 weeks ago"
git log --until="2023-12-31"

# Search commits
git log --grep="fix"                       # Search commit messages
git log -S "function_name"                 # Search for code changes
git log --author="John Doe"

# Show file changes
git log --stat                             # Show files changed
git log -p                                 # Show actual changes
git log --name-only                        # Show only file names

# Format output
git log --pretty=format:"%h - %an, %ar : %s"
git log --graph --pretty=format:"%C(yellow)%h%Creset -%C(red)%d%Creset %s %C(green)(%cr) %C(bold blue)<%an>%Creset"
```

## EXAMINING SPECIFIC COMMITS
```bash
# Show commit details
git show <commit-hash>
git show HEAD                              # Show last commit
git show HEAD~1                            # Show previous commit

# Show only files changed
git show --name-only <commit-hash>

# Show changes for specific file
git show <commit-hash>:<file-path>
```

## AMENDING COMMITS
```bash
# Amend last commit message
git commit --amend -m "New commit message"

# Amend last commit with new changes
git add <file>
git commit --amend                         # Opens editor
git commit --amend --no-edit               # Don't change message

# Change author of last commit
git commit --amend --author="New Author <email@example.com>"
```

## COMMIT REFERENCES
```bash
# Different ways to reference commits
HEAD                                       # Current commit
HEAD~1 or HEAD^                           # Parent commit
HEAD~2 or HEAD^^                          # Grandparent commit
HEAD~n                                     # n commits back

# Using commit hash
git show abc123f                          # Full or partial hash

# Using branch/tag names
git show main
git show v1.0.0
```

## INTERACTIVE COMMIT CREATION
```bash
# Interactive add (choose what to stage)
git add -i

# Patch mode (stage parts of files)
git add -p <file>
git commit -p                             # Add and commit in patch mode

# Interactive commit for multiple files
git add .
git commit -v                             # Shows diff in commit editor
```

## COMMIT HOOKS
Git hooks are scripts that run automatically on certain Git events:

### PRE-COMMIT HOOK EXAMPLE
```bash
#!/bin/sh
# .git/hooks/pre-commit

# Run tests before commit
npm test

# Check code style
npm run lint

# If any command fails, prevent commit
if [ $? -ne 0 ]; then
    echo "Tests or linting failed. Commit aborted."
    exit 1
fi
```

### COMMIT MESSAGE HOOK EXAMPLE
```bash
#!/bin/sh
# .git/hooks/commit-msg

# Check commit message format
commit_regex='^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .{1,50}'

if ! grep -qE "$commit_regex" "$1"; then
    echo "Invalid commit message format!"
    echo "Format: type(scope): description"
    exit 1
fi
```

## COMMIT SIGNING
```bash
# Set up GPG signing
git config --global user.signingkey <key-id>
git config --global commit.gpgsign true

# Sign individual commits
git commit -S -m "Signed commit"

# Verify signatures
git log --show-signature
git verify-commit <commit-hash>
```

## COMMIT STATISTICS
```bash
# Show commit count by author
git shortlog -sn

# Show commit activity
git log --stat --since="1 month ago"

# Show changes over time
git log --pretty=format:"%h %ad %s" --date=short --since="1 week ago"

# Files changed most often
git log --pretty=format: --name-only | sort | uniq -c | sort -rg | head -10
```

---

