Application Packages Notes
===========================

General
-------

Linux Application Containers:
- **AppImage**: Most portable, smallest size, fastest. Single file, no installation required. No repository or automatic updates. Community project.

- **Snap**: Largest Appstore, slowest container. Application must be installed via snapd. Single central appstore only, driven by Canonical.

- **FlatPak**: Smallest Appstore, larger files. No real advantage over AppImages except allowing for individual repositories, enabling automatic updates without central store.


AppImage
--------

### AppImage desktop integration

AppImageLauncher:
- Project: https://github.com/TheAssassin/AppImageLauncher
- Install as PPA:
  ```
  add-apt-repository ppa:appimagelauncher-team/stable
  apt update
  apt install appimagelauncher
  ```

### Read Information about AppImages

- Show commandline help
  ```
  MyAppImage.AppImage --appimage-help
  ```
- Extract content of AppImage
  ```
  MyAppImage.AppImage --appimage-extract
  ```
- Read update information
  ```
  MyAppImage.AppImage --appimage-updateinfo
  ```

### AppImage Format
- Specification of file format: https://github.com/AppImage/AppImageSpec/blob/master/draft.md
- Desktop file: .desktop file in AppImage root
- Icons: in `usr/share/icons/hicolor`
- Metadata: in `usr/share/metadata`
- Update information: ISO or ELF section, depending on file type 1 or 2

Snap
----

- Desktop menu files are installed in `/var/lib/snapd/desktop`. Make sure the path is in `$XDG_DATA_DIRS`.

