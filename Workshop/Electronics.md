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

### UART Bridges

- SC16IS750 /52/60/62: SPI to 1/2x UART, 4Mbps/15Mbps SPI
- MAX3107: SPI to UART
- XR20M1172: SPI to dual UART

Devices
-------

### Capacitors
- Multilayer Ceramic (MLCC): low ESR, low ESL; ideal for bypass; high dependency on temperature, ..


Development Boards, µCs
-----------------------

### Arduino
- Arduino Nano: ~7€ (compatible), 23€ (original)
    - AtMega 328P
    - USB: UART (only, uses HW UART)
    - IOs:

- Arduino Nano ESP32: 20€
    - ESP32-S3
    - 2x UART

- Arduino Nano Every: 15€
    - AtMega4809:

- Arduino Micro:
    - ATMega32U4: 32Kb Flash, 2Kb RAM, 1KB ROM, 16Mhz
    - 20 GPIO: 7 PWM, 12 Analog, 1x SPI, 1x I2C, 1x UART
    - Supports USB client (USB 2, 12Mbit, 6 endpoints; keyboard, mouse, ..)

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

- Modules
  - ESP32-WROOM-32: 4Mb Flash, 450Kb ROM, 520Kb RAM; 3x UART, 2x I2C, 1x I2S, 16x PWM, WIFI, Bluetooh

  - ESP32-S3-N16R8: 16Mb Flash, 8Mb RAM, 3x UART, 2x I2C, 2x I2S, 4x SPI, 8x PWM, USB 2.0 OTG, WIFI, Bluetooh

- ESP32 NodeMCU: ~7€
    - ESP32-WROOM-32
    - 1x ADC, 16x PWM, 1x SPI, 1x I2C, 3x UART (1 UART used for USB serial console, can be changed)
    - Only USB flashing+serial

- ESP32-S3 Devkit: 23€
    - Xtensa dualcore, 240Mhz, 512Kb RAM, 384Kb ROM, 8Mb Flash?
    - USB-to-UART, USB-OTG; JTAG!?
    - 2x I2S, 14x Touch
    - 4x SPI, 3x UART, 2x I2C
    - 45 GPIOs

### Raspberry PI Pico
Cheap, easily available

- Versions:
    - 'W': With Bluetooth, WLAN

- Raspberry PI 
    - RP2040: Dual Cortex-M0, 133Mhz, 32bit, 2Mb Flash, 256Kb RAM
    - 16x PWM, 2x I2C, 2x UART, 2x SPI, 1x ADC 12bit, 
    - Serial Wire Debug (SWD)
    - USB 1.1 Device/Host
    - 30 GPIOs @ 3.3V
 
- Raspberry PI Pico 2 W
    - RP2350A: Dual Cortex-M33 150Mhz, 32bit, 4Mb Flash, 512Kb RAM
    - Bluetooth, WLAN
    - 24x PWM, 2x I2C, 2x UART
    - 30 GPIOs @ 3.3V

#### ESP32 JTAG Debugging with PlatformIO
- When JTAG TDI pin is connected, upload via USB does not work. Upload via JTAG instead
  ```
  upload_tool = esp-prog
  ```
- To start debugging, use F5 (Start Debugging). Check output of "Debug Output" tab for progress or issues!
- platformio included debugger depends on python2.7. Workaround: build ESP-IDF debugger
  - Follow instructions from https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/linux-macos-setup.html
    ```
    sudo apt-get install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
    mkdir -p ~/esp
    cd ~/esp
    git clone -b v5.4.1 --recursive https://github.com/espressif/esp-idf.git
    cd ~/esp/esp-idf
    ./install.sh esp32
    cd ~/.platformio/packages/toolchain-xtensa-esp32/bin/
    mv xtensa-esp32-elf-gdb xtensa-esp32-elf-gdb.orig
    cp ~/.espressif/tools/xtensa-esp-elf-gdb/14.2_20240403/xtensa-esp-elf-gdb/bin/xtensa-esp32-elf-gdb .
    cp ~/.espressif/tools/xtensa-esp-elf-gdb/14.2_20240403/xtensa-esp-elf-gdb/bin/xtensa-esp-elf-gdb-no-python .
    cp ~/.espressif/tools/xtensa-esp-elf-gdb/14.2_20240403/xtensa-esp-elf-gdb/lib/xtensa_esp32.so ../lib/
    ```

