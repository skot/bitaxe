```
Closed Source is Antithetical to Bitcoin
```
# The bitaxeUltra
bitaxe is a fully open source hardware Bitcoin ASIC miner. Ultra is the 3rd major revision of the bitaxe that now includes the BM1366 ASIC from the S19XP
![bitaxeUltra assembled](doc/built.png)

## Goals
- **Standalone**: can mine directly to your pool over WiFi. No External computer needed.
- **Embedded**: low cost, low maintenance, high availability, high reliability, low power.
- **ASIC**: based on the very, very efficient BM1366 from Bitmain.
- **Versatile**: solo/pool mining, autotune power/heat/efficiency.
- **Open Source**: All design files are provided.

## Features
- **ESP32-S3-WROOM-1** wifi microcontroller on board
- **TI TPS40305** buck regulator steps down the 5V input to power the BM1366
- **Maxim DS4432U+** current DAC digitally adjusts the BM1366 core voltage from 0.04V to 2.4V
- **TI INA260** power meter measures the input voltage and current of the miner
- **Microchip EMC2101** PWM controls the fan and monitors tach output. Measuring internal die temp isn't working.
- 0.91" **SSD1306 OLED** I2C Display Module 

## BM1366

- The BM1366 is a undocumented SHA256 mining ASIC from Bitmain. It's mostly used in the Antminer S19XP
- Bitmain claims the BM1366 has 0.021J/GH efficiency
- The BM1366 is available (new) for around $15 each in small quantities.
- The BM1366 has a different footprint and pinout from the BM1397 and BM1387 in previous bitaxe.
- The BM1366 appears to roll more than just the nonce on the chip. This is great news, because it allows much longer serial chains of ASICs and new work doesn't need to be sent as often.

## Current Status
![bitaxeUltra running](doc/ultra_running.png)
- v1 hardware has been built, and it does mine! Quite well actually. Be sure to check the [issues](https://github.com/skot/bitaxe/issues) for known bugs, reworks and errata.
- This is an _advanced_ build! It's also still early days, so prolly not the best thing if you're just looking for a bitcoin miner to run.

## Hardware
- [BM1366 from NBTC on AliExpress](https://www.aliexpress.us/item/3256804709142138.html). I got the "AG" variant. Not really sure what the difference is.
- [40x40mm heatsink and 5V fan](https://www.aliexpress.com/item/2251832861666365.html) from a random AliExpress seller. At least half of these arrived broken in some way. But they are cheap and the working ones do keep the BM1387's nice and cool when used with some thermal compound.
    - Swap this fan with the [Noctua NF-A4x10 5V](https://noctua.at/en/products/fan/nf-a4x10-5v) 5V 4-Pin fan for a much more pleasant experience.
- The BM1366 serial port is 1.8V. These pins are broken out, but the main idea is to communicate with the BM1366 from the ESP32
- Level shifters to interface the 1.8V BM1366 with the 3.3V ESP32. These pins are also broken out.
- [KiCad 7](https://www.kicad.org) design files
- All of the parts on the board are listed in the KiCad BOM

## Software
- The [ESP-Miner](https://github.com/skot/ESP-Miner) firmware used for the BM1397-based bitaxe has been adapted for the BM1366 and the current main branch can support both.
- There is still work to be done on reverse engineering the BM1366 register map. If you can help, get in touch! 


## Power Supply Requirements
- [5VDC Power supply](https://www.amazon.com/BTF-LIGHTING-Plastic-Adapter-Transformer-WS2812B/dp/B01D8FM4N4). Should be capable of over 15W
    - **DO NOT** use the BS barrel to screw terminal adapter that comes with some of these power supplies. They have horrible series resistance and your bitaxe will not be happy!
    - Needs to connect with [spade-style connectors](https://www.amazon.com/gp/product/B01G4POUAU)

### ESP32 Programming Requirements
**New** on the bitaxeUltra: it has a USB port for programming the ESP32!

To program the onboard ESP32 the old way, you need the following tools;

- [ESP-Prog](https://www.digikey.com/en/products/detail/espressif-systems/ESP-PROG/10259352) ESP32 Programmer
- [TC2030-IDC-NL](https://www.tag-connect.com/product/tc2030-idc-nl) Tag Connect Cable
- [TC2030-CLIP](https://www.tag-connect.com/product/tc2030-retaining-clip-board-3-pack)
- This is best done with VSCode and the Espressif plugin

## Building
- Check out [building.md](building.md) for PCB ordering tips
- Check out [assembly.md](assembly.md) for assembly tips
