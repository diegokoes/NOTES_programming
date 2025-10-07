# LINUX > DISTROS > ARCH LINUX

## SUMMARY
> [!summary]
> Arch Linux installation and configuration guide

## THEORY

### INSTALLATION PROCESS

Basic installation commands:
- `setfont ter-132b` - Set larger font for installation
- `loadkeys es` - Set Spanish keyboard layout
- `timedatectl` - Check time settings
- `timedatectl set-timezone Europe/Madrid` - Set timezone

Network check:
- `ip link` - Check network interfaces
- `ping archlinux.org` - Test connectivity

### PARTITIONING

```bash
fdisk /dev/disktopartition
```

### FILE SYSTEM
- Recommended: [[linux_btrfs|Btrfs]]

### RECOMMENDED PROGRAMS

**Audio/Music:**
- [Tauon Music Box](https://aur.archlinux.org/packages/tauon-music-box)
- [SoulseekQt](https://aur.archlinux.org/packages/soulseekqt)
- `pacman -S quodlibet`

**Virtualization:**
- Use ext4 submodule for VMs

## QUESTIONS

> [!tip]- What's the recommended font size for installation?
> Use `ter-132b` for better visibility during installation

> [!warning]- Which file system should I use?
> Btrfs is recommended for its advanced features like snapshots and compression

- - -
