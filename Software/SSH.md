SSH Configuration Notes
=======================

Various Trips and Tricks
------------------------

- Disconnect a hanging SSH terminal: Press `Enter`, `~`, `.`.


Setup SSH keys for password-less login
--------------------------------------

Setup SSH key authentication:
- Generate SSH keys
  ```
  # Enter passphrase for key
  ssh-keygen -t ed25519 -C "`whoami`@`hostname`" -f ~/.ssh/github_id_ed25519
  ```
- Upload `.pub` file to server (add to `~/.ssh/authorized_keys`)
  ```
  ssh-copy-id ~/.ssh/github_id_ed25519 user@server.com
  ```
- Use keys for host connections, add to `~/.ssh/config`: 
  ```
  Host github, github.com
      Hostname github.com
      User git
      IdentityFile ~/.ssh/github_id_ed25519
  ```

Setup SSH agent for password-less login:
- Install prerequisites
  ```
  apt install ksshaskpass
  ```
- Load SSH keys automatically on interactive login via KDE konsole, using kwallet to store the ssh-key passphrases. Add to `~/.zshrc`:
  ```
  SSH_ASKPASS=ksshaskpass
  export SSH_ASKPASS

  # Add ssh keys to ssh-agent, but only if we have ssh-agent with kwallet running
  if [ -f "$SSH_AUTH_SOCK" -a ! -z "$DISPLAY" ]; then
      # Just update keys every time on interactive login
      #   if [ "`ssh-add -l`" = "" ]; then
      # Make sure we use ksshaskpass, take TTY away from ssh-add by redirecting stdin/stdout
      ssh-add ~/.ssh/github_id_ed25519 </dev/null 2>/dev/null
  fi
  ```
- Note: KWallet must be installed, with `kwallet5_pam` module (automatically installed with Kubuntu) to store passwords
  in default wallet protected by login credentials.
- For terminals without KWallet (remote server, ..), use:
  ```
  # start SSH agent, set environment variables to agent instance
  eval $(ssh-agent)
  # Add keys to agent
  ssh-add ~/.ssh/github_id_ed25519
  ```

**Alternative (without ssh-agent):** Use ControlMaster to reuse SSH connections. 
This requires fewer logins and makes creating multiple connections faster:
- Edit `~/.ssh/config`:
  ```
  Host *
      # Path for control sockets. Make sure ~/tmp exists!
      ControlPath ~/tmp/master-%r@%h:%p
      # Keep connection open for some time after the master exits
      ControlPersist 10m
      
  Host server.com
      # Try to use an existing connection, otherwise create a new connection
      ControlMaster auto
      IdentityFile ~/.ssh/server_id_rsa
  ```

Locale settings with SSH terminals
----------------------------------

- Comment out the following in `/etc/ssh/ssh_config` on Debian/Ubuntu clients to avoid confusing servers that do not support the client locale:
  ```
  # Make sure the following is *not* set to not confuse servers not supporting this client's locale:
  # SendEnv LANG LC_*
  ```
  or, add to `~/.ssh/config`:
  ```
  Host *
      SendEnv -LANG -LC_*
  ```

Setup SSH server for root login for backup
------------------------------------------

- Edit `/etc/sshd/sshd_config`:
  ```
  # Do not allow root login generally
  PermitRootLogin no
  # Allow pubkey authentication
  PubkeyAuthentication yes

  # ...
  # !! Add the Match sections at the *end* of sshd_config !!

  # Allow executing commands via public key from specific hosts
  #
  # Note: Match with "Host" requires UseDNS yes
  #Match Host myserver.home
  Match Address 192.168.1.1
    PermitRootLogin forced-commands-only
  ```
- Create an `only` script that only executes the specified commands:
  ```
  # Create /usr/local/bin/only script
  cat <<'EOF' >> /usr/local/bin/only
  #!/bin/sh
  cmds="$@"
  set -- $SSH_ORIGINAL_COMMAND
  for allowed in $cmds; do
      if [ "$allowed" = "$1" ]; then
          exec "$@"
          exit $?
      fi
  done    
  echo "Command denied: $@" >&2
  exit 1
  EOF
  
  # Make executable
  chmod 755 /usr/local/bin/only
  ```
- Add the following to `/root/.ssh/authorized_keys` to allow only to execute `rdiff-backup` via root ssh-key login:
  ```
  command="only rdiff-backup" ssh-rsa [ssh-key ... ] root@myserver.home
  ```
