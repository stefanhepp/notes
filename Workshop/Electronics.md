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

Development Boards, µCs
-----------------------

### Arduino
- Arduino Nano: ~7€ (compatible), 23€ (original)
    - AtMega 328P
    - IOs
    - UARTs:

- Arduino Nano ESP32: 20€
    - ESP32-S3

- Arduino Nano Every: 15€
    - AtMega4809:

- Arduino Uno: 22€
    - AtMega 328P

### Teensy
More stable and optimized libraries than ESP32?, more power and more IOs, but no JTAG!

- Teensy 4.0: 28€
    - ARM Cortex M7, 600Mhz, 1Mb RAM (512k frei), 2Mb Flash, FPU, RTC
    - USB host?, USB client (USB 2.0 480Mbit)
    - No JTAG!
    - 3x SPI, 7x UART, 3x I2C
    - 3x CAN, 2x I2S, 1x SPDIF
    - 40x GPIO, 31x PWM, 14x analog inputs

- Teensy 4.1: 38€
    - ARM Cortex M7, 600Mhz, 1Mb RAM (512k frei), 8Mb Flash, FPU, RTC
    - Ethernet, USB Host, USB client
    - No JTAG!
    - 3x SPI, 8x UART, 1x I2C
    - 3x CAN, 2x I2S, 1x SPDIF
    - 42x GPIO: 35x PWM, 18x analog inputs
    - SD Card
    - Flash Upgrade: +128Mbit; RAM Upgrade: +8Mb

### ESP32
WIFI, JTAG, cheap, but not as powerful, lots of variants and clones

- ESP32 NodeMCU: ~7€
    - ESP32-WROOM-32
    - 1x ADC, 1x PWM, 1x SPI, 1x I2C, 1x UART

- ESP32-S3 Devkit: 23€
    - Xtensa dualcore, 240Mhz, 512Kb RAM, 384Kb ROM, 8Mb Flash?
    - USB-to-UART, USB-OTG; JTAG!?
    - 2x I2S, 14x Touch
    - 4x SPI, 3x UART, 2x I2C
    - 45 GPIOs

