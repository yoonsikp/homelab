# ZFS Tips

## Analyze Disks
Use `skdump` to analyze disks and SMART data

## Install ZFS
https://wiki.alpinelinux.org/wiki/ZFS
Enable scrubbing service and cron job.

## Generate key
```
dd if=/dev/urandom of=key bs=32 count=1

xxd -p key | tr -d '\n' && echo
```

## Generate Pool
zpool will create gpt partition and align it to 2048 * 512 byte sector. Even if is 4096 byte physical sector. Creating a mirror pool of sda3 and sdb3 
```
zpool create -o ashift=12 \
   -O acltype=posixacl -O dnodesize=auto -O normalization=formD \
   -O relatime=on -O xattr=sa -O encryption=aes-256-gcm \
   -O keylocation=prompt -O keyformat=hex \
   rust mirror sda3 sdb3
     
```

## Generate zvol
```
zfs create rust/server_vms
chown yoonsik:yoonsik /rust/server_vms
chown -hR yoonsik:yoonsik /rust/server_vms
```

## Loading and unloading key + mounting
```
zfs load-key tank
zfs mount tank
zfs mount -a

zfs umount tank
zfs unload-key tank
```

## Recieving encrypted ZFS content over ssh (no unlock needed)
```
sudo zfs send -Rw tank/vms | ssh root@198.57.27.183 'cat - > test'

ssh root@198.57.27.183 'cat test' | sudo zfs recv tank/vms

```

# Ensure consistent imports (important for NVMe)
```
sudo zpool import -d /dev/disk/by-id -aN
```

