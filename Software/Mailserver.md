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


