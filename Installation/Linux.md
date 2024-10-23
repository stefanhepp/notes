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
apt install blender spacenavd audacity vlc kdenlive gimp inkscape
apt install kdevelop clang clang-tidy cppcheck cmake cmake-gui git gitk kdiff3
apt install python3-pip python3-serial python3-numpy python3-scipy python3-opencv python3-tk python3-pil.imagetk
```

### Replace Firefox snap with apt on Ubuntu
```
sudo apt purge firefox
sudo snap remove firefox
rm -rf ~/Downloads/firefox.tmp
sudo add-apt-repository ppa:mozillateam/ppa
sudo apt update
echo '
Package: *
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001
' | sudo tee /etc/apt/preferences.d/mozilla-firefox
echo 'Unattended-Upgrade::Allowed-Origins:: "LP-PPA-mozillateam:${distro_codename}";' | sudo tee /etc/apt/apt.conf.d/51unattended-upgrades-firefox
sudo apt install firefox
```

### Onedrive Sync

- Do not install Debian/Ubuntu `onedrive` package (outdated)! 
- Follow instructions here to install: https://github.com/abraunegg/onedrive/blob/master/docs/ubuntu-package-install.md
- As user, run `onedrive`, follow instructions to login
- Enable running OneDrive sync in background: Run as user:
  - `systemctl --user enable onedrive`
  - `systemctl --user start onedrive`

### Install Nvidia drivers (Ubuntu)
```
# Install matching nvidia drivers
# Make sure to use linux-image-nvidia kernel for booting
sudo ubuntu-drivers list
sudo ubuntu-drivers install
```

Debian Server Installation
--------------------------

### Install some basic tools

```
# Installs 'lsb_release' tool
apt install lsb-release
```


### Upgrade to new release

Following instructions from: https://www.debian.org/releases/bookworm/amd64/release-notes/ch-upgrading.en.html

- Cleanup existing installation
    ```
    # Update to latest release
    apt upgrade
    apt upgrade
    # Backup apt lists
    dpkg --get-selections '*' > ~/dpkg_selections.txt
    # Find leftover temp config files
    find /etc -name '*.dpkg-*' -o -name '*.ucf-*' -o -name '*.merge-error'
    # Ensure correct GPG keys verification
    apt install gpgv
    # Check for unclean installation
    dpkg --audit
    apt-mark showhold
    ```
- Replace distribution name to new release or `stable` in `/etc/apt/sources.list`
- Run upgrade
    ```
    # Record the outputs
    script -t 2>~/upgrade-bookwormstep.time -a ~/upgrade-bookwormstep.script
    # Upgrade
    apt update
    apt full-upgrade
    # Cleanup
    apt autoremove
    ```

### Firewall Setup

Basic firewall setup via `ufw`:
- Create application port configurations in `/etc/ufw/application.d/`
- Enable applications by name via `ufw allow <application>`
- Enable firewall via `ufw enable`
