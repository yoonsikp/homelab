`/etc/postfix/main.cf`

```

mydestination = $myhostname, naut.ca, cloud.naut.ca, localhost.naut.ca, , localhost


smtpd_tls_security_level = encrypt
smtp_tls_security_level = encrypt

###

home_mailbox = Maildir/

#### SASL ####
## specify SASL type ##
smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
smtpd_sasl_local_domain = $myhostname
smtpd_sasl_security_options = noanonymous

smtpd_sasl_auth_enable = yes
smtpd_recipient_restrictions = permit_mynetworks, permit_sasl_authenticated,
                               reject_unauth_destination, check_policy_service unix:private/policyd-spf,
                               reject_rbl_client zen.spamhaus.org

#### SSL/TLS parameters ####
smtpd_tls_auth_only = yes
smtpd_tls_loglevel = 1
smtpd_tls_dh1024_param_file = /etc/ssl/certs/dhparams.pem

#sudo nano /etc/postfix/main.cf

## force email client high security ##
smtp_tls_protocols = !SSLv2,!SSLv3,!TLSv1,!TLSv1.1
smtp_tls_ciphers = high
smtp_tls_mandatory_protocols = !SSLv2,!SSLv3,!TLSv1,!TLSv1.1
smtp_tls_mandatory_ciphers = high

smtpd_tls_protocols = !SSLv2,!SSLv3,!TLSv1,!TLSv1.1
smtpd_tls_ciphers = high
smtpd_tls_mandatory_protocols = !SSLv2,!SSLv3,!TLSv1,!TLSv1.1
smtpd_tls_mandatory_ciphers = high

tls_preempt_cipherlist = yes


smtp_tls_exclude_ciphers = aNULL, eNULL, PSK, SHA, SHA256, SHA384, DSS, AESCCM
smtpd_tls_exclude_ciphers = aNULL, eNULL, PSK, SHA, SHA256, SHA384, DSS, AESCCM

smtp_tls_mandatory_exclude_ciphers = aNULL, eNULL, PSK, SHA, SHA256, SHA384, DSS, AESCCM
smtpd_tls_mandatory_exclude_ciphers = aNULL, eNULL, PSK, SHA, SHA256, SHA384, DSS, AESCCM


# DKIM
milter_default_action = accept
milter_protocol = 2
smtpd_milters = inet:localhost:8891
non_smtpd_milters = inet:localhost:8891

policyd-spf_time_limit = 3600

delay_warning_time=4h
confirm_delay_cleared=yes
message_size_limit = 52428800

smtpd_hide_client_session = yes

smtp_bind_address=173.211.12.1
smtp_bind_address6=2602:ffc5:200:100::1

```

`sudo nano /etc/postfix/master.cf`

```
smtp      inet  n       -       y       -       -       smtpd
  -o smtpd_sasl_auth_enable=no

submissions     inet  n       -       y       -       -       smtpd
  -o syslog_name=postfix/submissions
  -o smtpd_tls_wrappermode=yes
  -o smtpd_sasl_auth_enable=yes
  -o smtpd_recipient_restrictions=permit_sasl_authenticated,reject
  

policyd-spf  unix  -       n       n       -       0       spawn
    user=policyd-spf argv=/usr/bin/policyd-spf
```
