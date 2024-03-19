Electronics Notes
=================

Power Supply
------------

### 78xx Linear Regulators

- L7805CV: 1.5A
- TA7805: 1A
- 78L05: 0.1A 

Decoupling: Use 0.33uF tantal at input, 0.1uF tantal at output. Use 22uF elko at input, 10uF elko at output for buffering. 

Logic ICs
---------

### CMOS vs TTL

- CMOS (4xxx series):
  - Low power consumption
  - Can work at different voltage levels (3V, 5V, ..) but input levels not tolerant to lower voltage devices
- TTL (74xx series):
  - Fixed voltage levels
  - Higher power consumption

### 74xx Series

- 74LSxx: 3.3V compatible, high power consumption, fast
- 74HCxx: 5V only, lower power consumption
- 74HCTxx: 3.3V compatible, lower power consumption 

### RS232

- MAX232: 5V compatible dual driver, 120kbit
- MAX232E: 5V compatible dual driver, 250kbit
- MAX3232: 3-5V dual driver
- ICL3232: pin-compatible replacement for MAX3232, cheaper

Charge pump capacitors should be chosen larger than nominal values (factor 2) to compensate for degradation over time.
