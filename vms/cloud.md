# cloud
VM Network IP: 172.16.1.2/24

## Specs
- Public IP: 173.211.12.1/32

## Software
- UFW
- ACME Let’s Encrypt
- WireGuard
  - Local VPN
  - Public IP
- Dovecot
- Postfix
  - main.cf:`smtp_address_preference = ipv4`
- Radicale
- Nginx
  - Radicale
  - Ghost
  - MTA-STS
  - Vaultwarden
  - Jellyfin
- Borg Backup
- idevicebackup2
- virtiofs passthrough of backups

```
# /etc/fstab
rust_backups    /rust/backups   virtiofs        rw,noatime,_netdev,nofail
```

## Failover Networking
- cellular modem 

```
#!/usr/bin/env bash
set -euf

sudo ip neigh add proxy 172.16.0.130 dev enp4s0
sudo ip neigh add proxy 172.16.0.131 dev enp4s0
sudo ip neigh add proxy 172.16.0.132 dev enp4s0

if sudo mbim-network /dev/cdc-wdm0 status | grep "Status: activated"; then
    exit
fi
sudo mbim-network /dev/cdc-wdm0 start
echo ***
```
