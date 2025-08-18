# Router
Router: WRT1200AC

OS: OpenWRT (https://openwrt.org/toh/linksys/wrt1200ac)

Initial Install: factory firmware

Upgrade: sysupgrade firmware

## Settings

DHCP:
- disable dns servers from upstream in wan interface
- disable ntp servers from dhcp
- disable ipv6 dns
- disabled lan dns in dhcp
- don't change domain, or else local hostname resolution fails
- set hostname `macbook-air-f3` to static IP

Hostname:
- shady:172.16.0.254

WiFi:
- Enable WMM
- Enable Country Code
- Lower WiFi Transmit Power

Unbound:

- Guide: https://openwrt.org/docs/guide-user/services/dns/dot_unbound
- install ca bundle, unbound, luci
- add new zone with cloudflare in luci
- change unbound port
- Supplement: https://github.com/openwrt/packages/blob/openwrt-19.07/net/unbound/files/README.md
- Cloudflare config:
```
forward-addr: 1.1.1.1@853#cloudflare-dns.com
forward-addr: 1.0.0.1@853#cloudflare-dns.com
```
- Change local DNS to 127.0.0.1:5353
