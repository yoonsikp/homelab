`sudo nano /etc/fail2ban/jail.d/defaults-debian.conf`

```
[DEFAULT]
banaction = nftables
banaction_allports = nftables[type=allports]
bantime  = 60m
findtime  = 10m
maxretry = 5

[sshd]
backend = systemd
journalmatch = _SYSTEMD_UNIT=ssh.service + _COMM=sshd
enabled = true

[postfix-sasl]
enabled = true
filter = postfix[mode=auth]
port = smtp,465,submission
logpath = /var/log/mail.log
maxretry = 3

[dovecot]
enabled = true
filter = dovecot
logpath = /var/log/mail.log
maxretry = 5





```
