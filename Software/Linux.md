Linux Tipps and Tricks
========================

Mount Samba Shares as User
--------------------------

Install 'smb4k', mount via GUI

Sleep, Wakeup, Power Management
-------------------------------

Find out what prevents going to sleep
```
# Systemd inhibitors, not being helpful...
systemd-inhibit --list

# Get KDE inhibitors (PowerDevil?)
dbus-send --print-reply --dest=org.freedesktop.PowerManagement /org/kde/Solid/PowerManagement/PolicyAgent org.kde.Solid.PowerManagement.PolicyAgent.ListInhibitions

```

See power consumption, power states, ..
```
powertop
```

