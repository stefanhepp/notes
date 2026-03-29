RC Electronics
==============

Protocols
---------

### Air Protocols

#### Spektrum

Protocols: DSM2, DSMX

Modules:
- Multi-Module: 
  - 'DSM' protocol: DSM2 1F/2F, DSMX 1F/2F
  - Channel order of transmitter must match Multi-module channel order.
    Multi-module will always reorder to TAER order for receivers.

#### FrSky

Protocols:
  - ACCST v1: Supports D8 (V, D receivers), LR12 (L receives), D16 (X, XM, S, RX receivers) 
  - ACCST v2: Supports D16, fixes noise issues. 
    - D8, LR12 mode supported in ISRM modules?
  - ACCESS: Supports OTA, 24 channels. Not supported by Multi-module.

Modules: 
  - XJT Internal module
    - ACCST module, supports D8, LR12, D16 (v1), or D16 (v2 firmware)
  - Multi-Module
    - FrSkyD: D8 (ACCST v1)
    - FrSkyL: LR12 (ACCST v1)
    - FrSkyX: D16 (ACCST v1)
    - FrSkyX2: D16 (ACCST v2)


#### ExpressLRS

Protocols
  - 
  - ELRS backpack
  - 

### Serial Bus Protocols

#### SRXL2

Spektrum, bidirectional bus for channels and telemetry.
- 115200 baud, 8N1, LSB, non-inverted UART
- Idle high with pullup, requires open-collector TX on bus

#### S.Bus

Unidirectional bus for channels only
- 100000 baud, 8E2, LSB, non-inverted (FrSky) or inverted (Futaba) UART

#### S.Port (SmartPort)

Unidirectional bus for telemetry sensors

Used in combination with S.Bus (F.Port is Futaba S.Bus (inverted UART) and S.Port)

#### CRSF (CrossFire)

Bidirectional bus for channels and telemetry.
- Transmitter module: Single-wire half-duplex 400kbaud, 8N1, 3.3V
- Receiver: dual-wire full-duplex 416666 baud, 8N1, 3.3V

Used by CrossFire BS and ExpressLRS.

https://github.com/tbs-fpv/tbs-crsf-spec/blob/main/crsf.md

#### MAVLINK

Bidirectional telemetry bus
