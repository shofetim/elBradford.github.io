---
published: true
date: '2018-02-09 10:00:00'
title: 'How to: Set up Zoho SMTP on Postfix'
permalink: /2018/zoho-postfix-smtp-relay
author: bradford
layout: post
categories:
  - Technology
  - Scripts
  - How To
tags:
  - Zoho
  - Postfix
  - SMTP
comments: true
---
I'm setting up my Proxmox server to send me email alerts. I want to use my Zoho email, and this is how I did it.

```bash
$ sudo nano /etc/postfix/sasl_passwd
```

Add your smtp server, email address, and password (ideally an application-specific password generated in your Zoho control panel):

```
smtp.zoho.com email@address.com:password
```

Now hash your postfix password and set proper permissions on the original:

```bash
$ sudo postmap hash:/etc/postfix/sasl_passwd
$ sudo chmod 600 /etc/postfix/sasl_passwd
```

Now, edit `/etc/postfix/main.cf` and add the following lines:

```
relayhost = smtp.zoho.com:465
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_security_options =
smtp_tls_wrappermode = yes
smtp_tls_security_level = encrypt
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_tls_CAfile = /etc/ssl/certs/Entrust_Root_Certification_Authority.pem
smtp_tls_session_cache_database = btree:/var/lib/postfix/smtp_tls_session_cache
smtp_tls_session_cache_timeout = 3600s
sender_canonical_classes = envelope_sender, header_sender
sender_canonical_maps = regexp:/etc/postfix/sender_canonical
smtp_header_checks = regexp:/etc/postfix/smtp_header_checks
```

Create `/etc/postfix/sender_canonical` and put in your Zoho email address:

```
/.+/ email@address.com
```

Create `/etc/postfix/smtp_header_checks` and put in your Zoho email address

```
/From:.*/ REPLACE From: email@address.com
```

Optionally, if you want to customize the name that the email is coming from, try:

```
/From:.*/ REPLACE From: Dumbledore <email@address.com>
```

Finally, reload postfix and send a test message:

```bash
$ sudo postfix reload
$ echo "test message" | mail -s "test subject" another@email.com
```

Voila! If you have issues, check your logs in `/var/log/syslog` and `/var/log/mail.info`

### Resources
[Serverfault: Forcing the from address when postfix relays over smtp](https://serverfault.com/questions/147921/forcing-the-from-address-when-postfix-relays-over-smtp)