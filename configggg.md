# CachyOS System Maintenance Guide

> **INFO:** This guide covers maintaining your CachyOS system, including system updates, AUR packages, and development environments like Python and Rust.

## System Updates

### Standard System Update
```bash
# Update your system (pacman packages)
sudo pacman -Syu

# If mirrors are having issues, refresh them first
sudo pacman-mirrors -f && sudo pacman -Syyu
```

### Partial Updates
```bash
# Check for updates without installing
pacman -Syup

# Update only specific packages
sudo pacman -S package1 package2
```

### After Major Updates
```bash
# Check for pacnew/pacsave files after updates
find /etc -name "*.pacnew" -o -name "*.pacsave"

# Compare and merge changes
sudo pacdiff
```

## AUR Package Management

### Using Paru (AUR Helper)
```bash
# Update all packages including AUR
paru -Syu

# Clean build cache
paru -Sc

# Search AUR packages
paru -Ss package_name
```

### Using Yay (Alternative AUR Helper)
```bash
# Update all packages including AUR
yay -Syu

# Clean build cache
yay -Sc
```

### Manual AUR Updates
```bash
# Navigate to AUR package directory
cd ~/path/to/aur/package

# Pull latest PKGBUILD
git pull

# Rebuild and install
makepkg -si
```

## Development Environment Updates

### Python Environment (pyenv)

```bash
# Update pyenv itself
cd ~/.pyenv && git pull

# Update plugins (if installed)
cd ~/.pyenv/plugins/pyenv-virtualenv && git pull

# List installed Python versions
pyenv versions

# Install new Python version
pyenv install 3.12.1

# Set global version
pyenv global 3.12.1

# Update pip in current environment
pip install --upgrade pip

# Update all packages in current environment
pip list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install -U
```

### Rust Environment

```bash
# Update Rust toolchain
rustup update

# Check current Rust setup
rustup show

# Update installed cargo packages
cargo install-update -a
# If not installed: cargo install cargo-update

# List installed cargo binaries
cargo install --list

# Check outdated dependencies in projects
cargo outdated
# If not installed: cargo install cargo-outdated
```

## System Maintenance

### Cleaning Package Cache
```bash
# Remove all cached packages that aren't installed
sudo pacman -Sc

# Remove ALL cached packages (even installed ones)
sudo pacman -Scc

# Keep only the most recent version of each package
paccache -r
```

### Finding and Removing Orphaned Packages
```bash
# List orphaned packages
pacman -Qtdq

# Remove orphaned packages
sudo pacman -Rns $(pacman -Qtdq)
```

### System Cleanup
```bash
# Clear journal logs older than 2 weeks
sudo journalctl --vacuum-time=2weeks

# Check disk usage
df -h

# Find large files/directories
du -h --max-depth=1 /path/to/check | sort -hr
```

### Check for Failed Services
```bash
# List failed systemd services
systemctl --failed

# Check system boot time
systemd-analyze

# Check service startup times
systemd-analyze blame
```

## Special CachyOS Commands

```bash
# Optimize CachyOS settings
sudo cachyos-settings-manager

# Update CachyOS specific packages
sudo cachyos-update

# Manage kernels
sudo cachyos-kernel-manager
```

## Hardware Checks

```bash
# Check for system errors
dmesg | grep -i error

# Monitor system temperatures
sensors

# Check disk health
sudo smartctl -a /dev/sda
```

Remember to back up important data regularly, especially before major system updates!

- - -