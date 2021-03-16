# Systemd
- custom services are stored here: `/etc/systemd/system`
- analyse security of service unit: `systemd-analyze security <service_unit>`
- restart a service: `systemctl restart <application_name>.service`
- show logs of a service: `journalctl -u <application_name>.service`
