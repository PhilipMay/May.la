# Linux Security

## Disable Portmap Service
You should disable the portmap service. You can check if it is available:

```text
$ rpcinfo -T udp -p <ip>
   program vers proto   port  service
    100000    4   tcp    111  portmapper
    100000    3   tcp    111  portmapper
    100000    2   tcp    111  portmapper
    100000    4   udp    111  portmapper
    100000    3   udp    111  portmapper
    100000    2   udp    111  portmapper
```

Or check with `systemctl list-units --type=service`.

Uninstall with: `apt-get remove rpcbind --auto-remove`

## Disable IPv6
- see https://www.itzgeek.com/how-tos/linux/debian/how-to-disable-ipv6-on-debian-9-ubuntu-16-04.html
- edit `/etc/sysctl.d/70-disable-ipv6.conf`
- add `net.ipv6.conf.all.disable_ipv6 = 1`
- restart: `sysctl --system`
- check with `ifconfig` - something like `inet6 2a01:4f8:1c1c:3240::1` should not be printed

## sshd security
- see https://www.digitalocean.com/community/tutorials/how-to-harden-openssh-on-ubuntu-18-04
- see https://community.hetzner.com/tutorials/securing-ssh

```text
# own settings
PasswordAuthentication no
PermitEmptyPasswords no
```
