# linux > file systems > btrfs

## Summary
> [!summary]
> Btrfs is a modern copy-on-write file system with advanced features like snapshots, compression, and RAID support

## Theory

### Key Features
- **Copy-on-Write (CoW)** - Data integrity and efficient snapshots
- **Snapshots** - Point-in-time filesystem states
- **Compression** - Built-in transparent compression
- **RAID Support** - Software RAID 0, 1, 5, 6, 10
- **Subvolumes** - Separate filesystem trees within a single Btrfs filesystem

### Use Cases
- System installations with snapshot capability
- Development environments
- Data storage with built-in backup features

## Questions

> [!tip]- When should I use Btrfs over Ext4?
> Use Btrfs when you need snapshots, compression, or advanced RAID features. Stick with Ext4 for maximum stability and compatibility.

> [!warning]- Is Btrfs stable for production use?
> Btrfs is stable for most use cases, but avoid RAID 5/6 in production environments.

## Resources

- [Arch Linux Btrfs Wiki](https://wiki.archlinux.org/title/Btrfs)

- - -
#linux

