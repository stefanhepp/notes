CNC 3018 Pro Settings
=====================

Configuration
-------------


Feed rates
----------



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

