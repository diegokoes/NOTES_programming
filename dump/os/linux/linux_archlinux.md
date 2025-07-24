# linux > distros > arch linux

## Summary
> [!summary]
> Arch Linux installation and configuration guide

## Theory

### Installation Process

Basic installation commands:
- `setfont ter-132b` - Set larger font for installation
- `loadkeys es` - Set Spanish keyboard layout
- `timedatectl` - Check time settings
- `timedatectl set-timezone Europe/Madrid` - Set timezone

Network check:
- `ip link` - Check network interfaces
- `ping archlinux.org` - Test connectivity

### Partitioning

```bash
fdisk /dev/disktopartition
```

### File System
- Recommended: [[linux_btrfs|Btrfs]]

### Recommended Programs

**Audio/Music:**
- [Tauon Music Box](https://aur.archlinux.org/packages/tauon-music-box)
- [SoulseekQt](https://aur.archlinux.org/packages/soulseekqt)
- `pacman -S quodlibet`

**Virtualization:**
- Use ext4 submodule for VMs

## Questions

> [!tip]- What's the recommended font size for installation?
> Use `ter-132b` for better visibility during installation

> [!warning]- Which file system should I use?
> Btrfs is recommended for its advanced features like snapshots and compression

- - -
#linux