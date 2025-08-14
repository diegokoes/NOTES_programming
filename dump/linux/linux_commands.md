# LINUX > COMMANDS
## CHMOD & CHOWN
> on NTFS they don't behave like on EXT4, mount options co
## LSBLK
- `lsblk -o`: 
## MOUNT
- 
## FILE SYSTEM
- `grep -w [filesystem] /proc/filesystems`: check if it's recognized
## FIREWALL
- 
## KERNEL
- `uname -r`: to see version

## MODULES
- `modinfo [name]`: see if it exists
## RAM
- `free -h` - Display memory usage
## STORAGE
- `df -h` : show disk usage
### SWAP
- `zramctl` : Display zram device information
- `swapon --show`: show swap priority and usage 


### SYSTEM ANALYSIS

**systemd analysis:**
```bash
# Boot time analysis with plot
systemd-analyze plot

# Show services startup time
systemd-analyze blame
```

## COMMAND REFERENCE

### D
- `df -h` - Display filesystem disk space usage
- `dysk` - Modern disk usage analyzer



### S
- `swapon --show` - Display swap usage and priority
- `systemd-analyze` - Analyze system boot performance



- - -



