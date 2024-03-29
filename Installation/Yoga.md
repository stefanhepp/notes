Lenovo X1 Yoga Gen1 Installation
================================

General
-------
- Enable touch scrolling in Firefox: add to `/etc/security/pam_env.conf`:
  ```
  MOZ_USE_XINPUT2 DEFAULT=1 OVERRIDE=1
  ```

Kubuntu 22.04 LTS
-----------------

- Install Plasma Desktop
  ```
  apt install plasma-workspace-wayland kwin-wayland
  ```
- Kubuntu not waking up from sleep
  - Select "Discrete Chip" (TPM 1.2) as "Security Chip" in BIOS security settings, or disable "Security Chip" fully (but disabling prevents Windows PIN login from working)
  - Use UEFI boot mode
- Display scaling
  - Setting display scaling to 150% (or any fractional value != 100%, 200%,..) leads to artifacts in Konsole,..
  - Instead:
    - *Fonts* settings -> Enable *Force Font DPI*, set to 144 DPI
    - In Konsole, Profile Settings -> *Appearance* -> *Miscancellous* -> Set *Line Spacing* to 1px.
- Sleep mode, Fan Control
  - `apt install tlp thinkfan tp-smapi-dkms`
- Pen Notetaking App: "Write"
  - Download from: http://www.styluslabs.com/download/
  - Unpack to ~/Apps folder, run `setup.sh`
- Tablet mode
  - Install Maliit Keyboard: TBD
