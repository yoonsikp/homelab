
1. You want DNS from DHCP
sudo apt install isc-dhcp-client rdnssd
sudo apt install openresolv (already installed usually)

2. You want recursive DNS
sudo apt install unbound
sudo systemctl disable --now unbound-resolvconf.service

nano /etc/resolvconf.conf
```

resolv_conf=/etc/resolv.conf
# If you run a local name server, you should uncomment the below line and
# configure your subscribers configuration files below.
name_servers=127.0.0.1
```

resolvconf -u

nano /etc/unbound/unbound.conf.d/local.conf
```
server:
#  outgoing-interface: 173.211.12.1
#  outgoing-interface: 172.16.0.70
#  outgoing-interface: 2602:ffc5:200:100::1
  edns-buffer-size: 1232

  qname-minimisation: yes
  cache-max-negative-ttl: 1
  prefetch: yes
  target-fetch-policy: "3 2 1 0 0"
  hide-identity: yes
  hide-version: yes
  interface-automatic: yes

  access-control: 127.0.0.0/8 allow
  access-control: ::1 allow
  access-control: 200::/7 allow

  log-queries: no
  log-replies: no
  log-tag-queryreply: no
  verbosity: 0
  use-syslog: yes
  discard-timeout: 4900

  domain-insecure: lan
  private-domain: lan

  local-zone: local always_nxdomain

  domain-insecure: router
  private-domain: router
  local-zone: router static
  local-data: "router. 300 IN A 172.16.0.1"

  local-data: "nas.naut.ca. 300 IN AAAA 206:ce06:5aa7:13df:e565:9a0c:b936:7c48"
  local-data: "cloud.naut.ca. 300 IN AAAA 2602:ffc5:200:100::1"
  local-data: "cloud.naut.ca. 300 IN A 173.211.12.1"
  local-data: "ghost.naut.ca. 300 IN CNAME cloud.naut.ca."
  ```
