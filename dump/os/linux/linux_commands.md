# linux > commands
## chmod & chown
> on NTFS they don't behave like on EXT4, mount options co
## lsblk
- `lsblk -o`: 

## Mount
- 
## File System
- `grep -w [filesystem] /proc/filesystems`: check if it's recognized

## Firewall
- 
## Kernel

- `uname -r`: to see version

## Modules
- `modinfo [name]`: see if it exists
## RAM
- `free -h` - Display memory usage
- `zramctl` - Display zram device information
## Process Management

**Background processes:**
```bash
# Run command in background
command &

# Prevent termination on logout
nohup command &

# Run in background and disown from shell
command & disown
```

### Memory and Storage

**Memory monitoring:**
```bash
# See real zram usage
sudo zramctl

# Check swap priority and usage
swapon --show

# See memory/swap usage
free -h
```

**Disk usage:**
```bash
# Show disk usage
df -h

# Advanced disk usage tool
dysk
```

### System Analysis

**systemd analysis:**
```bash
# Boot time analysis with plot
systemd-analyze plot

# Show services startup time
systemd-analyze blame
```

## Command Reference

### D
- `df -h` - Display filesystem disk space usage
- `dysk` - Modern disk usage analyzer



### S
- `swapon --show` - Display swap usage and priority
- `systemd-analyze` - Analyze system boot performance



- - -
#linux


