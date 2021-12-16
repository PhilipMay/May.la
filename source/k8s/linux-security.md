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
