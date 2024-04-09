PCB Design Notes
================

Board Design Rules
------------------

### Prototyping Boards
- Hole diameter: 1.02mm
- Pad size: 2.2mm

### CNC milled PCBs
- Tools
    - V-carve bit: 0.1mm tip, 30°
    - Drill: 0.8mm / 1.0mm
    - End mill: 1mm-2mm
- All through-holes: 0.8mm / 1.0mm
- Vias
    - Hole Diameter: 0.8mm (0.6mm wires)
    - Pad size: 2mm
- Traces
    - Trace minimum size: 0.3mm
    - Trace-to-trace clearance: 0.2mm
    - Pour-to-trace clearance: 0.3mm

### Manufactured boards (Aisler, PCBWay, JLCPcb)


Flatcam Settings
----------------
- Isolation Routing
    - Open Gerber
        - Isoliation Routing
        - Generate Geomentry
        - Generate CNC Job
    - Cut Z: -0.1mm
    - Travel Z: 2mm
    - End Travel Z: 5mm
    - Feedrate: 120mm
    - Z feedrate: 60mm
    - Spindle Speed: 12000
    - Passes: 4
    - Overlap: 10% (minimum; increase if necessary)
    - Combine (generate single path containing all passes)
- Drilling
    - Open Excellon
        - Drilling Tool
    - Cut Z: -2.0mm
    - Feedrate Z: 300mm
    - Spindle Speed: 6000
    - Tool Change:
- Edge Cutting
    - Open Gerber
        - Cutout Tool > Generate Geometry (with tabs)
        - Generate CNC Job
    - Tool Diameter: 
    - Cut Z: -1.8mm
    - Multidepth: 0.6mm
    - Tabs: TB 



PCB Manufacturers
-----------------

### PCBWay
- 5 boards minimum
- CNC, 3D printing
- KiCAD support: Push Plugin

### JLCPcb
- 5 boards minimum
- 3D printing

### Aisler
- Webpage: https://aisler.net/
- 7x5 2-layer board: 16€ + Shipping (LogoIX; 1-3 days Germany + LogoIX)
- 3 boards minimum
- Board colors: Green
- KiCAD support: native, Push Plugin
- LibrePCB support: native

