1. Install rsyslog
```bash
sudo apt update
sudo apt install rsyslog logrotate
```
Enable and start it:

```bash
sudo systemctl enable --now rsyslog
```
2. Enable journald forwarding
Create a configuration override:


```bash
sudo mkdir -p /etc/systemd/journald.conf.d
sudo nano /etc/systemd/journald.conf.d/rsyslog.conf
```
Add:


```ini
[Journal]
Storage=volatile
ForwardToSyslog=yes
```

3. Restart journald

```bash
sudo systemctl restart systemd-journald
```4. Verify both services

```bash
systemctl status systemd-journald rsyslog
```
Check the syslog socket:


```bash
sudo ss -xlpn | grep journal
```
5. Test logging

```bash
logger "Debian 13 logging test"
```
Then:

```bash
journalctl -t logger -n 5
```
And:

```bash
grep "Debian 13 logging test" /var/log/syslog
```

 The message appears in both journald and /var/log/syslog
