Linux Tipps and Tricks
========================

Various Commandline Tricks
--------------------------

- See progress of mv, cp, .. commands: `progress -m`

Mount Samba Shares as User
--------------------------

Install 'smb4k', mount via GUI

Sleep, Wakeup, Power Management
-------------------------------

Enable Wake-on-LAN for magic packets (option `g`):
```
ethtool -s enps6s0 wol g
```

Suspend computer from commandline:
```
systemctl suspend
```

Find out what prevents going to sleep
```
# Systemd inhibitors, not being helpful...
systemd-inhibit --list

# Get KDE inhibitors (PowerDevil?)
dbus-send --print-reply --dest=org.freedesktop.PowerManagement /org/kde/Solid/PowerManagement/PolicyAgent org.kde.Solid.PowerManagement.PolicyAgent.ListInhibitions

```

See power consumption, power states, .. and change wakeup sources
```
powertop
```


