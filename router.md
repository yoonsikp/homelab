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
- enable delegate IPv6 prefix
- assignment length 64
- RA server mode + local ipv6 dns server
- RA Flags other config, enable slaac
- enable packet steering
- enable software flow offloading

Hostname:
- shady:172.16.0.254

WiFi:
- Enable WMM
- Enable Country Code
- Lower WiFi Transmit Power

Unbound:
- install unbound, luci
- enable fallback for AFXR 
- change unbound port to 5353
- Change dnsmasq upstream DNS to 127.0.0.1:5353
