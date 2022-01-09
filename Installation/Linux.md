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


