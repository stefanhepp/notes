LDAP
====

- Connect to slapd as local user: Use `-Y EXTERNAL -H ldapi:///` to authenticate via current user password.
- List all configuration: `ldapsearch -x -H "ldap://ldap.home" -D "cn=admin,cn=config" -W -b "cn=config"
- Dump, edit and update config file database:
  ```
  # Dump the file database to .ldif
  slapcat -F /etc/ldap/slapd.d -n0 -l backup_config.ldif
  # Edit .ldif manually
  # Remove old database
  rm -r /etc/ldap/slapd.d/*
  # Recreate the database
  slapadd -F /etc/ldap/slapd.d -l backup_config.ldif
  # Test the database
  slaptest -F /etc/ldap/slapd.d
  ```
- Install ldapsearch and friends: `apt install ldap-tools`

Authenticate via LDAP
---------------------

- Install `libnss-ldapd` and libpam-ldapd`
- Configure LDAP server and filters in `/etc/nslcd.conf`. See `man nslcd.conf` for options.
    - Use `pam_authz_search` to filter for service and host membership.
- Configure `/etc/nsswitch.conf` to use `files ldap` for `passwd`, `shadow` and `group`
- Check configuration of /etc/pam.d/common-* to check with pam_ldap. Should be configured automatically by pam-ldap.

