# vault

## Software
Alpine Linux, install with `export SWAP_SIZE=0`

```
apk add vaultwarden
apk add vaultwarden-web-vault
rc-update add vaultwarden

# disable logging
rc-update delete syslog boot
```


```
# cat /var/lib/vaultwarden/.env

## Also used as working directory for the ".env"
DATA_FOLDER=/var/lib/vaultwarden

## Enable this to true and install vaultwarden-web-vault to use it
WEB_VAULT_ENABLED=true
WEB_VAULT_FOLDER=/usr/share/webapps/vaultwarden-web
DISABLE_ICON_DOWNLOAD=true
SIGNUPS_ALLOWED=false

ROCKET_ADDRESS=::

```
