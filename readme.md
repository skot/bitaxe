# The bitaxeMax!
an early-stage experimental bitcoin mining machine. Uses a single Bitmain BM1397 ASIC (chip).

## BM1397
![BM1397 pic](doc/BM1397.png)

- The BM1397 is a undocumented SHA256 mining ASIC from Bitmain. It's mostly used in the Antminer S17
- Bitmain claims the BM1397 has 0.03J/GH efficiency
- The BM1397 is available (new) for around $20 each in low quantities from Aliexpress
    - I have seen some used BM1397 sellers; DYOR
        - [HYG SRong](https://www.aliexpress.us/item/3256804436095856.html) -- $4.66 @ QTY 10 w/ shipping
        - [BTC Zone](https://www.aliexpress.us/item/3256804305413883.html) -- $6 @ QTY 10 w/ shipping
        - [punuo-ic](https://www.aliexpress.us/item/3256804576938680.html) -- $6.16 @ QTY 1 w/ shipping

- The BM1397 has the same footprint as the BM1387, but a very different pinout.
    - It also has two "Modes" that change some of the signal pins around to make chaining easy

## Current Status
- I have not built up any or tested any of these PCBs yet. Parts and PCBs are supposed to arrive any day now..

## Hardware
- [BM1397 from random AliExpress seller](https://www.aliexpress.com/item/3256802274958527.html). I got the "AG" variant. Not really sure what the difference is.
- [40x40mm heatsink and 5V fan](https://www.aliexpress.com/item/2251832861666365.html) from a random AliExpress seller. At least half of these arrived broken in some way. But they are cheap and the working ones do keep the BM1387's nice and cool when used with some thermal compound.
- The BM1397 serial port is 1.8V. These pins are broken out, but the main idea is to communicate with the BM1397 from the ESP32
- I added level shifters for the interface with the [ESP32-C3](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/hw-reference/esp32c3/user-guide-devkitc-02.html#user-guide-c3-devkitc-02-v1-header-blocks) (Because it has 3.3V GPIOs)
- I added a NCT218 so that the BM1397 core temperature can be read out over I2C
    - NCT218 are pretty hard to come by these days. Maybe a replacement is needed.
- [KiCad 6](https://www.kicad.org) design files
- All of the parts on the board are listed in the KiCad BOM

## Software
- still TBD

## Connections
![](doc/render.png)

| Pin Name     | Description |
| ----------- | ----------- |
| VIN      | ASIC VIN - ASIC core voltage. Rumor has it this should be around 1.5V for the the BM1397. This needs more experimentation.       |
| +5V   | 5V input gets regulated to 1.8V and 0.8V for the BM1397. Also 5V for Fan output. Not much current required. If the ESP32 is powered by USB, you don't need to power this.       |
| +5V (Fan)   | 5V output for fan .       |
| SPD (Fan)   | Speed input from fan.       |
| VIO   | ESP32-C3 GPIO voltage (3.3V)       |
| RXI   | 1.8V Serial input        |
| TXO   | 1.8V Serial output        |
| RST   | Inverted reset pin to the first BM1397. Unsure how necessary this is.        |
| GND   | Common ground        |

## Goals
- Develop some firmware for the ESP32 to connect to a stratum server/pool over WiFi and get work for the mining ASIC
    - Firmware should monitor fan speed
    - Firmware should monitor BM1397 core temperature (via NCT218)
    - Firmware should tune each BM1397 by adjusting it's core voltage
- Put a high current, low voltage buck regulator on the board that the ESP32 can adjust the voltage of.
    - This might be something like the TPS40305. I've made a separate [prototype of that power supply](https://github.com/skot/TPS40305_Supply)
    - One way to adjust the output voltage of these power supplies is with a DAC. The regular ESP32 has one, but the ESP32-C3 doesn't :(
- Develop some software in Rust to getblocktemplate from a bitcoin node and communicate work to a fleet of bitaxes.
- add an onboard power sensor so we can set the hashrate (ASIC requency) and therefore current to extract maximum power from a solar panel.
