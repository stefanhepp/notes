CNC 3018 Pro Settings
=====================

Configuration
-------------

### Axes:
-  +X: left -> right
-  +Y: front -> back (relative to bed)
-  +Z: bottom -> top
-  Machine coordinate zero: right back top 
-  Home to zero -> machine coordinates always negative

### bCNC Configuration
```
Acceleration x:	20.0
Acceleration y: 20.0
Acceleration z: 20.0
Feed max x: 1800.0
Feed max y: 1800.0
Feed max z: 1800.0
Travel x: 306.0
Travel y: 198.0
Travel z: 61.0
Decimal digits: 4
Plotting arc accuracy: 0.01
Start up: G90
Spindle min: 0
Spindle max: 12000
DRO Zero Padding: 1
Header gcode: M3 S12000; G4 P3
Footer gcode: M5
```

### grbl Controller
```
$0 Step pulse time [us]: 10.0
$1 Step idle delay [ms]: 25
$2 Step port invert mask: 0
$3 Direction port invert mask: 2
$4 Step enable invert: off
$5 Limit pins invert: off
$6 Probe pin invert: off
$10 Status report (mask): 1
$11 Junction deviation [mm]: 1.0
$12 Arc tolerance [mm]: 0.002
$13 Report inches: off
$20 Soft limits: off
$21 Hard limits: on
$22 Homing cycle: on
$23 Homing direction invert [mask]: 0
$24 Homing feed [mm/min]: 25.0
$25 Homing seek [mm/min]: 500.0
$26 Homing debounce [ms]: 250
$27 Homing pull-off [mm]: 2.0
$30 Max spindle speed [RPM]: 12000.0
$31 Min spindle speed [RPM]: 0.0
$32 Laser mode enable: off
$100 X steps/mm: 800.0
$101 Y steps/mm: 800.0
$102 Z steps/mm: 800.0
$110 X max rate [mm/min]: 1800.0
$111 Y max rate [mm/min]: 1800.0
$112 Z max rate [mm/min]: 800.0
$120 X acceleration [mm/sec]: 20.0
$121 Y acceleration [mm/sec]: 20.0
$122 Z acceleration [mm/sec]: 20.0
$130 X max travel [mm]: 306.0
$131 Y max travel [mm]: 198.0
$132 Z max travel [mm]: 61.0
$140 X homing pull-off [mm]: 2.0
$141 Y homing pull-off [mm]: 2.0
$142 Z homing pull-off [mm]: 2.0
```
Config dump (`$$`):
```
$0=10
$1=25
$2=0
$3=2
$4=0
$5=0
$6=0
$10=1
$11=1.000
$12=0.002
$13=0
$20=0
$21=1
$22=1
$23=0
$24=25.000
$25=500.000
$26=250
$27=2.000
$30=12000
$31=0
$32=0
$100=800.000
$101=800.000
$102=800.000
$103=31.111
$110=1800.000
$111=1800.000
$112=800.000
$113=200.000
$120=20.000
$121=20.000
$122=20.000
$123=20.000
$130=306.000
$131=198.000
$132=61.000
$133=360.000
ok
$G
[GC:G0 G54 G17 G21 G90 G94 M5 M9 T0 F0 S0]
ok
```

G-Code
------
### 3018 G-Code settings
- Header
```
G21 ; set units to MM
G90 ; absolute distance mode
```
- Enable spindle
```
M3 S12000 ; Enable spindle, 12000rpm
G4 P3     ; Pause (dwell) for 3 seconds
```
- Footer
```
M5  ; spindle off
M30 ; program end
```

### G-Code commands
```
G0 X<x> Y<y> Z<z>  ; Rapid movement to position.
G1 X<x> Y<y> Z<z>  ; Move to position using feed speed. 

G4 P<time>         ; Pause (dwell) for <time> seconds.

G17    ; Select XY plane
G20    ; Use inches for units
G21    ; Use millimeters for units

G90    ; set absolute distance mode: G0,G1,.. command 
         coordinates are absolute values
G91    ; set relavite distance mode: G0,G1,.. command
         coordinates are offsets to current position

G94    ; Feed mode F<feed> in units per minute

G54, G55, ..; Select work coordinate system

M3 P<speed> ; Enable spindle with <speed> rpm
M5          ; Stop spindle
M30         ; program end
```

### Grbl commands
```
$$         ; Show settings
$<x>=<val> ; Set setting <x> to <val>

$N         ; Show startup codes
$N<x>=val  ; Set startup codes

$#         ; G-code parameters store the coordinate offset
           ; values for G54-G59 work coordinates, G28/G30 
           ; pre-defined positions, G92 coordinate offset, tool 
           ; length offsets
$G         ; Show G-Code parser state (coordinate mode, ..)
$I         ; Show build info

?          ; Query state
!          ; Feed Hold
```

Feed rates
----------
- Max Spindle Speed: 12000 RPM
- 4-flute end mill 3.175mm
    - Cut direction: Climb cut (left)

Milling parameters
==================

General rules
-------------
- Bits
    - Flutes
      Less flutes for larger chip load, less friction with lower feed rates

      Less flutes for faster cutting but less cleaner finish. Larger number of flutes for cleaner finish but slower cutting.

      Use lower number of flutes for heat-sensitive material like acrylic and aluminium!

    - Upcut versus downcut
      - Upcut: for faster cutting, removing bulk, but worse surface finish

        Note: fix large sheets in the center (sticky tape,..) to prevent lifting while cutting with upcut bit!

      - Straight flute: Easier to produce, cheaper, cleaner cut, but less material removed from the cut.

      - Downcut: for cleaner finish, but needs slower cutting

      - Compression bits: only useful with depth of cut lower than upcut length of bit

      - Corncob bits (many-cutting-surface bits): More robust design for smaller bits, but less material removal

    - Bit types
      - End mill: Flat top, for general purpose milling
      - Ball nose: For curved surface milling
      - Bull nose: Mix between ball nose and flat end mill
      - V-bit/engraving bit: Engraving, 3D relief cutting
      - Surfacing bit: Flatten spoilboard, flatten surfaces

- Chip load, feeds and speeds
    - Chip load should be large enough to transfer heat away.

    - Chip load = Feed Rate / (RPM * #cutting edges)
      Feed rate (ipm) = RPM * #cutting edges * chip load
      Speed (RPM) = Feed rate / #cutting edges * chip load

    - Assume fixed speed, tune chip load with feed rate

- Wood machining, rules of thumb
    - Default cut direction: climb cut

    - Pass depth: 1x - 2x bit diameter
      
      Example: 3.175mm bit: 3mm to 6mm depth for cutting

    - Plunge rate: 1/2 of feed rate

      Use ramping when plunging

    - V-bits feedrate
      1 flute: 40 ipm
      2 flute: 80 ipm

    - Chiploads (ipm) for 1/8" end mills
      - Hardwood: 0.003 - 0.005
      - Plywood:  0.004 - 0.006
      - MDF:      0.004 - 0.007
      - Soft Plastic: 0.003 - 0.006
      - Hard Plastic: 0.002 - 0.004

    - Feed rates for 1/8" end mills @14k RPM
      - 4 flutes:
        - Hardwood: 


Calibration
-----------
- Set steps per inch
  - Move axis first into same direction to remove backlash before starting travel
  - Steps per mm = (current steps per mm * target distance [mm]) / measured distance [mm]



TODOs
-----
- grbl
    - Flash new version 1.1h
    + grbl enable Soft Limits, stop without hard reset. 
    - Enable (require) homing after reset (?)
- GCode Sender (bCNC, ..)
    - Requirements
	- Windows, Linux support
	- Modern looking interface
	- No Javascript, Web GUI (only)
	+ Pendant support: Keyboard, Joystick, USB Pendant, Web GUI
	+ 3D Viewer, OpenGL, free rotation
	- Load + Append GCode
	- Check GCode
	- Probing: Z-probe, center finder, Edge finder, Corner finder
	+ Jog: Enter Coordinates (Go To), Jog by increments, Jog freely (continuous)
	+ Show current Feed + Speed rate
	- Read out Machine Position, Work position / Offset
	- 4th Axis support
	- Bounding Box, show max travel for job; Run bounding box travel at Max-Z
	- G40 Cutter Compensation configuration
	- Machine Configuration
    - Toolkits
	- Autoleveling
	    - Set dimensions + position, or define geometry
	    - Heightmap visualization
	- SVG/DXF Laser cutting
	- SVG/DXF Engraving
	- Isoliation milling (FlatCam)
	- 3D Toolpath preview (CAMotics)

