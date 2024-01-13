CAM Workflows
=============

### 3D Printing
- Design: FreeCAD, Blender, Fusion360, SolidWorks Community
    - Export to STL
- Generate GCode: PrusaSlicer
    - Export to USB stick

### PCB Manufacturing
- Design: KiCAD, Fusion360
    - Export to Gerber files
- Generate GCode: FlatCam
- Toolpath Simulation: CAMotics
- CNC Milling: bCNC
    - Use Autoleveling

### CNC Milling
- Design: FreeCAD (CAM Plugin/Path Workbench), Fusion360
    - Generate GCode Toolpath
- Toolpath Simulation: CAMotics
- CNC Milling: bCNC


### Engraving
- Design: Inkscape
    - Create SVG/DXF File
- V-Carve Path (GCode): Fusion360, F-Engrave
- CNC Engraving: bCNC

### Laser Cutting
- Design: Inkscape
    - Create SVG file for cut paths (hide engraving images)
    - Create PNG file for engraving (cut paths as light color for alignment, filter cut paths out)
- CNC Laser Cutting: LaserGRBL


Software
========

Used Software
| Software | Linux | Windows |
| -------- | ----- | ------- |
| FreeCAD  | x | x |
| Fusion360 |   | x | 
| KiCAD | x | x |
| Inkscape | x | x |
| FlatCam | x | x |
| F-Engrave | x | x |
| PrusaSlicer | x | x |
| CAMotics | x | x |
| bCNC | x | x |

FreeCAD
-------

### Installation on Linux: Use AppImage
- Does not start on Linux failing to load iris_dri.so or swrast_dri.so: Start with
  ```
  LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libstdc++.so.6 /path/to/FreeCAD.appimage
  ```

KiCAD
-----

### Installation on Linux

Install from KiCAD PPA to get the latest versions:
```
sudo add-apt-repository --yes ppa:kicad/kicad-7.0-releases
sudo apt update
sudo apt install --install-recommends kicad
```

### Plugins

- Push-to-Aisler, Push-to-PCBWay
    - Provided via official library manager

LibrePCB
--------

Sources: https://github.com/LibrePCB/LibrePCB

### Installation on Linux
- Install AppImage from https://librepcb.org/download/

bCNC
----

Sources: https://github.com/vlachoudis/bCNC

### Installation on Linux
```
pip install --upgrade bCNC
```

FlatCAM
-------

Sources: https://bitbucket.org/jpcgt/flatcam.git

### Installation on Linux
```
git clone https://bitbucket.org/jpcgt/flatcam.git
git checkout Beta
```
You may need to make the following changes to `setup_ubuntu.sh`:
- Install rasterio from `python3-rasterio` package instead of via pip
- Pin pip package version `vispy==0.9.0`

CAMotics
--------

Sources: https://github.com/CauldronDevelopmentLLC/CAMotics

### Installation on Linux
- Build from source: https://camotics.org/download.html#source-code
    - Remove unused (?) dependency on libssl1.1 from SConstruct
- Alternatively, follow the instructions from this page to install released dpkg: https://camotics.org/download.html#install
    - Requires fixed version of libv8 on Kubuntu 22.04

Software Wishlist, TODOs
------------------------

### bCNC, grbl
- grbl Simulator
- Set offset for current position numerically
- Detect misconfiguration of $$ configuration
  - Values >0, < INT\_MAX
- Software limits in bCNC
  - Show boundary of machine
  - do not send if moving outside area
  - Recover after SW limit
- Pendant support for Jogging
- Keyboard shortcut list, shortcuts for jogging, stop
- Grbl: 
  - Hard limit/soft limit: recover from limit, retain position

### KiCAD
- Open, manage multiple projects
- Design rules library: Load design rules from template, select system- and user- templates

