Mailserver Configuration Notes
==============================

General
-------

- Send mail from commandline
  ```
  apt install mailutils
  mail root
  ```

Mailserver Setup
----------------

Used Software:
- *Postfix*: SMTP mail forwarder and filter. Delivers mails to dovecot.
- *Dovecot*: IMAP server and manages storage of mails.
- *Roundcube*: Webmail server.
- *opendkim*: DKIM signature and validation

Common ports:
- 25/tcp: Postfix SMTP
- 465/tcp: Postfix SMTPS
- 587/tcp: Postfix Secure Submission
- 143/tcp: Dovecot IMAP
- 993/tcp: Dovecot Secure IMAP
- 4190/tcp: Managesieve protocol

Preferred setup:
- Mailboxes are managed (stored) by Dovecot. Mail is delivered to dovecot instead of stored by postfix directly.
  - Advantage: mail storage is handled exclusively by Dovecot. Access permissions are only required for Dovecot, not local users. Supports both local and virtual users.
  - Disadvantage: Local users have no direct access to mailboxes via commandline readers such as mutt.
- Access control is handled via Dovecot. Central configuration for accounts.

Certificates
------------

LetsEncrypt Setup:
- Add deploy-hook script in `/etc/letsencrypt/renewal-hooks/deploy` to restart postfix and dovecot after cert renewal.

Postfix
-------

- Setup a satellite server to forward local accounts mail to a mail server
  - Install postfix, configure as satellite server
  - Setup /etc/postfix/main.cf for TLS encryption and SASL authentication
  - Setup /etc/aliases to forward local accounts to the domainadmin email address
  - Run `postalias /etc/aliases` after editing the file

- Debug authentication or permission issues: Add `-v` to options for `submission`,.. in `master.cf`

Dovecot
-------


Mail Validation and Signature
-----------------------------

### SPF: Authenticate sender server via DNS entry
```
      TXT "v=spf1 mx ~all"
```

Syntax:
- 'mx' means, all MX records are valid senders
- '~all': soft check; mark other emails as suspicious
- '-all': hard check; reject other servers for this domain

### DKIM: Sign and verify email
```
default._domainkey TXT "v=DKIM1; h=sha256; k=rsa; p=<public key>"
```

- `default` is the selector
- Check with `dig default._domainkey.yourdomain.org`
- Use opendkim as postfix milter to sign and check emails
- See this page for instructions:
  https://gist.github.com/howyay/57982e6ba9eedd3a5662c518f1b985c7

### DMARC: Define rules for DKIM checked emails
```
_dmarc TXT "v=DMARC1; p=quarantine; rua=mailto:postmaster@yourdomain.org;"
```
