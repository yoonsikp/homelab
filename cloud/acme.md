export CF_Token="sdfsdfsdfljlbjkljlkjsdfoiwje"
export MAIL_TO="alerts@naut.ca"

acme.sh --set-default-ca --server letsencrypt

acme.sh --issue -d naut.ca -d '*.naut.ca' --keylength ec-384 --dns dns_cf --force

acme.sh --install-cert -d naut.ca -d '*.naut.ca' \
--key-file /home/acme/naut.ca/naut.ca.key \
--fullchain-file /home/acme/naut.ca/naut.ca.pem \
--reloadcmd "sudo systemctl reload postfix; sudo systemctl reload dovecot; sudo systemctl reload nginx" \
--ecc --force

(needs mailutils)
acme.sh --set-notify --notify-hook mail

mkdir -p /home/acme/naut.ca
chmod o+x /home/acme
chmod o+x /home/acme/naut.ca

sudo ln -s /home/acme/naut.ca/naut.ca.pem /etc/ssl/certs/naut.ca.pem
sudo ln -s /home/acme/naut.ca/naut.ca.key /etc/ssl/private/naut.ca.key


sudo nano /etc/sudoers.d/acme

acme ALL=(ALL) NOPASSWD: /usr/bin/systemctl reload nginx, /usr/bin/systemctl reload postfix, /usr/bin/systemctl reload dovecot

