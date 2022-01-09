Lenovo X1 Yoga Gen1 Installation
================================

Kubuntu 20.04 LTS
-----------------

- Display scaling
  - Setting display scaling to 150% (or any fractional value != 100%, 200%,..) leads to artifacts in Konsole,..
  - Instead:
    - *Fonts* settings -> Enable *Force Font DPI*, set to 144 DPI
    - In Konsole, Profile Settings -> *Appearance* -> *Miscancellous* -> Set *Line Spacing* to 1px.
- Sleep mode, Fan Control
  - `apt install tlp thinkfan`
  - ..
- Screen rotate
- Tablet mode
