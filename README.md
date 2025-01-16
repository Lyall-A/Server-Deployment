# Lyall's server deployment :D

## Packages
* ssh
* samba

## YES
* `sudo usermod -aG docker lyall` GIVE ME DOCKER PERMS
* Mount drives at /mnt/drive\<number>
* Place `Docker files` in home directory
* Place `smb.conf` in `/etc/samba`
* Place `sshd_config.conf` in `/etc/ssh`
* Place your public keys in `~/.ssh/authorized_keys`