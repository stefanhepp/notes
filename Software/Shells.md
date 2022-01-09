ZSH
===

Configuration files
-------------------

1. `zshenv`: Executed for all shells (login, interactive, others). Main use: set environment variables for all shells and scripts.
2. `zprofile`: Executed for login shells (`-zsh` or `zsh --login`). Can be both interactive or non-interactive logins (e.g., remote login via SSH). 
3. `zshrc`: Executed for interactive shells ("command line"; can be login or non-login session), e.g., Konsole will create interactive non-login sessions. Main use: set everything relevant for interactive command line.
4. `zlogin`: Similar to `zprofile`, but executed last.
5. `zlogout`: Executed on logout from a login session.


