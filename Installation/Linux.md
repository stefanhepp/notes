Linux Installation Notes
========================

Kubuntu Laptop/PC Installation
------------------------------

### Grub Setup

- Remember last choice
    - Edit `/etc/default/grub`:
      ```
      GRUB_DEFAULT=saved
      GRUB_SAVEDEFAULT=true
      ```
    - Run `update-grub`
- Disable memtest menu entries:
    - `chmod -x /etc/grub.d/20_memtest86+`
    - Run `update-grub`

### RTC System Clock

Set BIOS system clock to local time (Windows default) instead of UTC (Linux default), to avoid mismatch with Windows:
```
timedatectl set-local-rtc 1 --adjust-system-clock
```

### Standard Software Installation

Common software packages to install
```
apt install vim zsh screen ksshaskpass
apt install blender spacenavd musescore audacity vlc kdenlive gimp
apt install kdevelop clang clang-tidy cppcheck cmake cmake-gui git gitk kdiff3
apt install python3-pip python3-serial python3-numpy python3-scipy python3-opencv python3-tk python3-pil.imagetk
```

### KiCAD Installation

Install from KiCAD PPA to get the latest versions:
```
sudo add-apt-repository --yes ppa:kicad/kicad-6.0-releases
sudo apt update
sudo apt install --install-recommends kicad
```

### Onedrive Sync

- Do not install Debian/Ubuntu `onedrive` package (outdated)! 
- Follow instructions here to install: https://github.com/abraunegg/onedrive/blob/master/docs/ubuntu-package-install.md
- As user, run `onedrive`, follow instructions to login
- Enable running OneDrive sync in background: Run as user:
  - `systemctl --user enable onedrive`
  - `systemctl --user start onedrive`


Debian Server Installation
--------------------------

### Firewall Setup

Basic firewall setup via `ufw`:
- Create application port configurations in `/etc/ufw/application.d/`
- Enable applications by name via `ufw allow <application>`
- Enable firewall via `ufw enable`
