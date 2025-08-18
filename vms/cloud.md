# cloud
VM Network IP: 172.16.1.2/24

## Specs
- Public IP: 173.211.12.1/32

## Software
- ACME Let’s Encrypt
- WireGuard
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
