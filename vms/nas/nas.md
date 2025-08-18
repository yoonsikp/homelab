# nas
## Specs
- OpenWRT
- 4TB Lexar NM790 NVMe

## Software
- Samba
- Transmission Torrent Server

Mount drives:
```
opkg install kmod-fs-exfat block-mount  kmod-fs-ext4  luci-app-samba4

https://openwrt.org/docs/guide-user/storage/usb-drives

block detect | uci import fstab
uci set fstab.@mount[-1].enabled='1'
uci commit fstab

update LUCI - samba i.e. /mnt/sdb1

option options 'dmask=000,fmask=111,relatime'
```


OpenWRT:

```
socket options = IPTOS_LOWDELAY TCP_NODELAY SO_RCVBUF=524288 SO_SNDBUF=524288
force group = root
force user = root

kernel oplocks = yes
fruit:posix_rename = yes
fruit:zero_file_id = yes
fruit:wipe_intentionally_left_blank_rfork = yes 
fruit:delete_empty_adfiles = yes
fruit:nfs_aces = no
server min protocol = SMB3
server smb encrypt = off

# readdir_attr:aapl_max_access = no
#	force create mode = 666
#	force directory mode = 777
```


