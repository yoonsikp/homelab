# cloud

## Specs
- Public IP: 173.211.12.1/32

## Software
- UFW
```
To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere                  
25/tcp                     ALLOW       Anywhere                  
465/tcp                    ALLOW       Anywhere                  
993/tcp                    ALLOW       Anywhere                  
80/tcp                     ALLOW       Anywhere                  
443                        ALLOW       Anywhere                  
9001                       ALLOW       Anywhere                  
123/udp                    ALLOW       Anywhere                  
22/tcp (v6)                ALLOW       Anywhere (v6)             
25/tcp (v6)                ALLOW       Anywhere (v6)             
465/tcp (v6)               ALLOW       Anywhere (v6)             
993/tcp (v6)               ALLOW       Anywhere (v6)             
80/tcp (v6)                ALLOW       Anywhere (v6)             
443 (v6)                   ALLOW       Anywhere (v6)             
9001 (v6)                  ALLOW       Anywhere (v6)             
123/udp (v6)               ALLOW       Anywhere (v6)  
```
- chronyd
  - `allow 200::/7` 
- ACME Let’s Encrypt
  - /home/acme
  - See acme.txt
- WireGuard
  - Public IP
- Dovecot
  - protocol imap {  mail_max_userip_connections = 50  }
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
