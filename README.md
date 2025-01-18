# Lyall's server deployment :D
Assuming x64 with UEFI, a lot of this is based of [Arch installation guide](https://wiki.archlinux.org/title/Installation_guide)

## TODO:
* Missing WiFi driver, not sure if that means i'm missing a ton of drivers
* Doesn't resolve it's own hostname, causes issues with sudo

## Partitioning
* Use fdisk: `sudo fdisk /dev/sdb`
* If using UEFI:
  * Create new GPT partition table: `g`
  * Create new EFI partition: `n`, `+1G`, `t`, `EFI System`
  * Create new swap partition: `n`, `+8G`, `t`, `Linux swap`
  * Create new root partition: `n`, `t`, `Linux root (x86-64)`
* If using BIOS:
  * Create new MBR partition table: `o`
  * Create new swap partition: `n`, `+8G`, `t`, `Linux swap`
  * Create new root partition: `n`, `t`, `Linux root (x86-64)`
* Write changes and exit fdisk: `w`
* Setup EFI partition (if using UEFI; might need to install dosfstools): `sudo mkfs.fat -F32 /dev/sdb1` 
* Setup root partition: `sudo mkfs.ext4 /dev/sdb3`
* Setup swap partition: `sudo mkswap /dev/sdb2`

## Installing Debian
* Mount root partition: `sudo mount /dev/sdb3 /mnt`
* Install Debian with debootstrap: `sudo debootstrap stable /mnt http://deb.debian.org/debian/`
* Mount EFI partition (if using UEFI): `sudo mount /dev/sdb1 /mnt/boot`
* Mount `/dev`, `/proc` and `/sys`: `sudo mount --bind /dev /mnt/dev && sudo mount --bind /proc /mnt/proc && sudo mount --bind /sys /mnt/sys`
* Get UUID's for partitions: `sudo blkid /dev/sdb1 && sudo blkid /dev/sdb2 && sudo blkid /dev/sdb3`
* Enter chroot environment: `sudo chroot /mnt`
* Might need to do this to add `/sbin` to PATH: `export PATH=$PATH:/sbin`
* Add EFI partition to fstab file: `echo "UUID=C4E0-226F /boot vfat defaults 0 1" > /etc/fstab`
* Add swap partition to fstab file: `echo "UUID=59fa3589-2e62-4f1a-955e-13b031297746 none swap sw 0 0" >> /etc/fstab`
* Add root partition to fstab file: `echo "UUID=82d1ff6b-f060-4903-9b73-9e855bd24f9a / ext4 errors=remount-ro 0 1" >> /etc/fstab`
* Install Linux image: `apt-get install linux-image-amd64`

## Installing bootloader (GRUB)
* If using UEFI:
  * Install `grub-efi`
  * Install GRUB: `grub-install --target=x86_64-efi --efi-directory=/boot`
  * Update GRUB: `update-grub`
* If using BIOS:
  * Install `grub-pc`

## Setup
* Set hostname: `echo "server1" > /etc/hostname`
* Add user: `useradd -m -s /bin/bash lyall`
* Set user password: `passwd lyall`
* Install [packages](#packages)
* Give user sudo permissions: `usermod -aG sudo lyall`
* Setup Network Manager
* Exit chroot environment: `exit`
* Unmount everything: `sudo umount /mnt/dev; sudo umount /mnt/proc; sudo umount /mnt/sys; sudo umount /mnt/boot; sudo umount /mnt`
* Boot
* Make sure all services for packages are enabled
* Install Docker with `install_docker.sh`
* Give Docker permissions to user: `sudo usermod -aG docker lyall`
* Place `smb.conf` in `/etc/samba`
* Place `sshd_config.conf` in `/etc/ssh`
* Place public keys in `~/.ssh/authorized_keys`

## Extras
* Mount drives at `/mnt/drive<number>`
* Place `Docker files` in home directory

## Packages
* ssh
* samba
* sudo
* network-manager

## Useful packages
* glances
* neofetch