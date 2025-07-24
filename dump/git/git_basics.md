# Git Basics

## Installation and Configuration
```bash
# Set up your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# View configuration
git config --list
git config user.name
```

## Repository Initialization
```bash
# Create a new repository
git init

# Clone an existing repository
git clone <repository-url>
git clone <repository-url> <directory-name>
```

## Basic Workflow
```bash
# Check repository status
git status

# Add files to staging area
git add <filename>
git add .                    # Add all files
git add *.js                 # Add all JavaScript files
git add -A                   # Add all changes (including deletions)

# Commit changes
git commit -m "Commit message"
git commit -am "Add and commit in one step"  # For tracked files only

# View commit history
git log
git log --oneline
git log --graph --oneline --all
```

## File Operations
```bash
# Show differences
git diff                     # Working directory vs staging area
git diff --staged           # Staging area vs last commit
git diff HEAD               # Working directory vs last commit

# Remove files
git rm <filename>           # Remove from working directory and stage deletion
git rm --cached <filename>  # Remove from Git but keep in working directory

# Move/rename files
git mv <old-name> <new-name>
```

## Viewing Changes and History
```bash
# Show specific commit
git show <commit-hash>

# Show file history
git log -- <filename>
git log -p <filename>       # Show changes in each commit

# Blame - see who changed what
git blame <filename>
```

## Ignoring Files
Create a `.gitignore` file to specify which files Git should ignore:
```
# Dependencies
node_modules/
*.log

# Build outputs
dist/
build/

# Environment files
.env
.env.local

# IDE files
.vscode/
.idea/

# OS files
.DS_Store
Thumbs.db
```

## Useful Aliases
```bash
# Set up useful shortcuts
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph --all"
```

---
#git
