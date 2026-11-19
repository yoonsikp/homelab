```
apt-get install opendkim opendkim-tools


nano /etc/opendkim.conf

Domain                  example.com
KeyFile                 /etc/postfix/dkim.key
Selector                mail
Socket                  inet:8891@localhost




opendkim-genkey -t -s mail -d naut.ca

mv mail.private /etc/postfix/dkim.key

cat mail.txt
```
