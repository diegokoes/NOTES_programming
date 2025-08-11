arch: amd64
cores: 2
features: keyctl=1,nesting=1
hostname: qbittorrent
memory: 2048
mp0: /mnt/laptop,mp=/mnt/laptop
net0: name=eth0,bridge=vmbr0,gw=192.168.1.1,hwaddr=BC:24:11:3A:73:F0,ip=192.168.1.23/24
onboot: 1
ostype: debian
rootfs: local-lvm:vm-105-disk-0,size=8G
swap: 512
tags: community-script;torrent
unprivileged: 1

fstab

# <file system> <mount point> <type> <options> <dump> <pass>
/dev/pve/root / ext4 errors=remount-ro 0 1
UUID=E081-4044 /boot/efi vfat defaults 0 1
/dev/pve/swap none swap sw 0 0
proc /proc proc defaults 0 0
//calm.local/lxcshare /mnt/laptop cifs guest,uid=100000,gid=100000,vers=3.1.1,x-sy>

