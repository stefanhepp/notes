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

### Installation on Linux, Windows
```
pip install --upgrade bCNC
```

### Toolchange (M6 command)
- In "Probe" menu, select "Tool" command
    - Set Policy to "Manual tool change (WCO)"
        - WCO will adjust world coordinates to new tool length
        - TLO will adjust TLO offset, keep World coordinates the same
    - Pause Before and After
    - Set Tool change position and Probe position
    - Set Distance: positive value, probe *down* from from Probe position (e.g, distance "10mm" will probe downwards from Probe position)
- For details, see https://github.com/vlachoudis/bCNC/wiki/Tool-Change

FlatCAM
-------

Sources: https://bitbucket.org/jpcgt/flatcam.git

### Installation on Linux
- Installation from original repository:
    ```
    git clone https://bitbucket.org/jpcgt/flatcam.git
    cd flatcam
    git checkout Beta
    pip install -r requirements.txt
    ```
    You may need to make the following changes to `setup_ubuntu.sh`:
    - Install rasterio from `python3-rasterio` package instead of via pip
    - Pin pip package version `vispy==0.9.0`
- Installation from my repository
    ```
    git clone git@github.com:stefanhepp/flatcam.git
    cd flatcam
    git checkout Beta
    pip install -r requirements.txt
    ```

### Installation on Windows
- Get flatcam from github
    ```
    git clone git@github.com:stefanhepp/flatcam.git
    cd flatcam
    git checkout Beta
    ```
- Install Miniforge3
    - Recommended to create a new environment: make sure PyQt6 is not installed in the same environment.
- Inside Miniforge shell, run
    ```
    conda create -n flatcam
    conda activate flatcam
    conda install gdal
    cd flatcam
    pip install -r requirements.txt
    ```

CAMotics
--------

Sources: https://github.com/CauldronDevelopmentLLC/CAMotics

### Installation on Linux
- Build from source: https://camotics.org/download.html#source-code
    - Remove unused (?) dependency on libssl1.1 from SConstruct
- Alternatively, follow the instructions from this page to install released dpkg: https://camotics.org/download.html#install
    - Requires fixed version of libv8 on Kubuntu 22.04

Firmware
--------
### grbl
- URL: https://github.com/gnea/grbl
- AtMega328p

### grblHAL
- URL: https://github.com/grblHAL
- 32bit processors, simulator
- Breakout boards: https://www.grbl.org/breakboards

### uCNC
- URL: https://github.com/Paciente8159/uCNC

Software Wishlist, TODOs
------------------------

### bCNC, grbl
- Show tool message for tool change
- Move to position: Enter position manually (WPOS / MPOS)
- 3D Visualization
- grbl Simulator
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

### Candle
- Bugfix: Stop probing with jog stop
- Stop/Pause button: Stop probing, jogging, running
- UI Layout
  - Settings buttins: Open File, Settings, Enable Laser
  - General buttons: Pause, Stop, Unlock, Reset, Home, Safe Position
  - User Buttons
  - Movement buttons: Zero Z, Zero XY, Set Pos, Move To, Reset Pos, Restore Origin; Move to XY origin; Scan boundary
  - Jogging controls: always visible
  - Tooboxes:
    - Heightmap
    - Probe: Probe XY, Probe Z, set Probe offset; goto probe position
    - Tool Change: Set probe position, set tool change position (get from current pos), set M6 mode
  - Larger console
  - Grbl Settings
- Enable/Disable Laser mode
- Heightmap: Fix grid preview?!
- Movement
  - Set current WPOS manually
  - Move to position, enter position (absolute G90 or relative G91, rapid G0 or normal G1)
- TLO support
- Tool Change M6 Support
  - Ignore, Adjust Z, Adjust TLO, Send M6, Custom Script
  - Set Probe position, Manual Toolchange position
- Probing
  - Toolbox
  - Adjust Z probe
- Visualization: Spinning -> change color tool

Candle Forks
- https://github.com/elhernes/Candle: RPN, Macros, Pendant support, some PRs merged, LCD numbers
- https://github.com/Schildkroet/Candle2: alive project, copied ~5years ago (no fork)
- https://github.com/stribor/Candle: OSX, new Qt support?
- https://github.com/mar0x/Candle: mostly OSX build fixes?
- https://github.com/Denvi/Candle/tree/Experimental: Moveable panels, Plugins
- https://github.com/etet100/G-Pilot-Formerly-Candle: Heavy rewrite, overloaded, virtual grbl target

### KiCAD
- Open, manage multiple projects

