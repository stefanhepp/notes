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
- CNC Milling: bCNC
    - Use Autoleveling

### CNC Milling
- Design: FreeCAD (CAM Plugin), Fusion360
    - Generate GCode Toolpath
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


