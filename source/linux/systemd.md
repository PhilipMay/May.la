# Systemd
  - custom services are stored here: `/etc/systemd/system`

## Analyse Security of Service Unit
``` bash
systemd-analyze security <service_unit>
```

## systemctl
  - restart a service: `systemctl restart <application_name>.service`
  - show logs of a service: `journalctl -u <application_name>.service`
