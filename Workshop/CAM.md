CAM Workflows
=============

### 3D Printing
- Design: FreeCAD, Blender, Fusion360, SolidWorks Community
    - Export to STL
- Generate GCode: PrusaSlicer
    - Export to USB stick

### PCB Manufacturing
- Design: KiCAD, Fusion360, LibrePCB
    - Export to Gerber files
- Generate GCode: FlatCam
- Toolpath Simulation: CAMotics
- CNC Milling: bCNC
    - Use Autoleveling

### CNC Milling
- Design: FreeCAD (CAM Plugin), Fusion360
    - Generate GCode Toolpath
- Toolpath Simulation: CAMotics
- CNC Milling: bCNC


### Laser Cutting, Engraving
- Design: Inkscape
- CNC Engraving: bCNC


Software
========

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
sudo add-apt-repository --yes ppa:kicad/kicad-6.0-releases
sudo apt update
sudo apt install --install-recommends kicad
```

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



