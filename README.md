# Lyall's server deployment :D

## Packages
* ssh
* samba
* network-manager

## Useful packages
* glances
* neofetch

## INSTRUCTIONSSS
* Install packages
* Make sure all services for packages are enabled
* Install Docker with `install_docker.sh`
* `sudo usermod -aG docker lyall` GIVE ME DOCKER PERMS
* Mount drives at `/mnt/drive<number>`
* Place `Docker files` in home directory
* Place `smb.conf` in `/etc/samba`
* Place `sshd_config.conf` in `/etc/ssh`
* Place public keys in `~/.ssh/authorized_keys`