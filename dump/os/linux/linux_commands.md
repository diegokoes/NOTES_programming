# linux > commands

## Summary
> [!summary]
> Essential Linux commands reference

## Theory

### Process Management

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

### F
- `free -h` - Display memory usage

### S
- `swapon --show` - Display swap usage and priority
- `systemd-analyze` - Analyze system boot performance

### Z
- `zramctl` - Display zram device information

## Questions

> [!tip]- What's the difference between nohup and disown?
> - `nohup command &` - Prevents process from receiving SIGHUP signal
> - `command & disown` - Removes job from shell's job table, preventing SIGHUP

- - -
#linux


