PCB Design Notes
================

Board Design Rules
------------------

### Prototyping Boards
- Hole diameter: 1.02mm
- Pad size: 2.2mm (0.6mm annular ring)

### CNC milled PCBs
- Tools
    - V-carve bit: 0.1mm tip, 30°
    - Drill: 0.8mm / 1.0mm
    - End mill: 2mm
- All through-holes: 0.8mm / 1.0mm

### PCB Milling, routing through pin headers
- Vias
    - Hole Diameter: 0.8mm (0.6mm wires)
    - Pad size: 1.7mm
    - Pad minimum annulars: 0.35mm
- Traces
    - Trace minimum size: 0.28mm
    - Minimum clearance: 0.27mm
    - Minimum connection width: 0.25mm
- Required tool
    - V-bit, 30degree, 0.1mm
    - Flatcam: 0.22mm V-Dia, -0.1mm Cut-Z

### PCB Milling, routing through DIP sockets but not pin headers
- Vias
    - Hole Diameter: 0.8mm (0.6mm wires)
    - Pad size: 1.8mm
    - Pad minimum annulars: 0.4mm
- Traces
    - Trace minimum size: 0.3mm
    - Minimum clearance: 0.32mm
    - Minimum connection width: 0.3mm
- Required tool
    - V-bit, 30 degree, 0.1mm
    - Flatcam: 0.25mm V-Dia, -0.1mm Cut-Z

### Manufactured boards (Aisler, PCBWay, JLCPcb)


Flatcam Settings
----------------
- Isolation Routing (V-bit, 30degree, 0.1-0.2mm)
    - Open Gerber
    - Isoliation Routing
        - Tool: Delete default 0.1mm tool
        - Tool: Pick from DB: Select tool, or create new tool
            - Right click on the list, Add to DB
            - Set tool settings: V-bit 30degree 0.1mm/0.2mm
            - Right click on the tool in the list; Save Settings!
        - Transfer the Tool
        - Enable Combine (generate single path containing all passes)
    - Generate Geometry
        - Preprocessor: grbl_11
    - Generate CNC Job
- Drilling (0.8-1.0mm drills)
    - Open Excellon
    - Drilling Tool
        - Select <2mm holes; avoid selecting duplicate holes
        - Cut Z: -2.3mm
        - Travel Z: 2mm
        - Feedrate Z: 30mm
        - Spindle Speed: 12000
        - Apply to all tools!
        - Tool Change: yes
        - Tool Change Z: 25mm
        - End Move Z: 25mm
        - Preprocessor: grbl_11
    - Generate CNC Job
    - Save CNC Code
- Milling larger holes and slots (2mm endmill)
    - Double-click Excellon file
    - Utilities -> Milling Geometry
    - Select >2mm holes in Drill tools table
    - Milling Diameter: 2.000
    - Mill Drills
    - In Geometry Object, select Pick from DB -> Endmill 2mm
        - Transfer Settings
- Edge Cutting
    - Open Gerber
    - Cutout Tool
        - Pick Tool from DB: Endmill 2mm (see settings above)
        - Gaps: LR (or what is appropriate)
    - Generate Geometry (any form)
        - Pick tool from DB: Endmill 2mm
        - Preprocessor: grbl_11
    - Generate CNC Job

### Flatcam Tool Database
- Tool Settings: V-Bit 0.1mm 30degree
    - Tool Description
        - Diameter: autocalculated, do not change!
            - Increase the tip size to increase the diameter, otherwise the generated paths will cut into the PCB tracks!
        - Operation: Isolation
    - Isolation Parameters
        - Isolation passes: 3
        - Overlap: 20%
        - Milling Type: Climb
        - Isolation Type: Full
    - Tool Parameters
        - Shape: V
        - V-Dia: 0.19
            - Adjust V-Dia so that the calculated diameter is 0.01mm smaller than the minimum clearance of the PCB
            - Increase overlap to remove any leftover areas if necessary
        - Tool Type: Iso (for isolation routing!)
        - Cut Z: -0.12
        - Travel Z: 2mm
        - Feedrate: 120mm
        - Z feedrate: 60mm
        - FR rapids: 1500mm
        - Spindle Speed: 12000
- Tool Settings: Endmill 2mm
    - Diameter: 2mm
    - Operation: General
    - Cutout parameters
        - Gap Size: 4mm
        - Gap Type: Thin
        - Depth: -1.00mm
        - Tool Diameter: 2mm
    - Milling Parameters
        - Shape: C4 (4-flute endmill)
        - Tool Type: Finish
        - Cut Z: -2.3mm
        - Multidepth: Yes
        - DPP: 0.8mm
        - Travel Z: 2.0mm
        - Feedrate XY: 120mm
        - Feedrate Z: 60mm
        - FR Rapids: 1500mm
        - Spindle Speed: 12000


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

