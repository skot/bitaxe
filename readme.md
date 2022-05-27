# The bitaxe
![](doc/assembled.png)
an experimental bitcoin mining machine. Uses 2x Bitmain BM1387 ASICs. Should be able to hash at around 130GH/s. Needs a lot of work!

## BM1387
![bm1387 pic](doc/bm1387.png)

- The BM1387 is a undocumented SHA256 mining ASIC from Bitmain. It's mostly used in the Antminer S9
- Bitmain claims the BM1387 has 0.098j/Gh efficiency
- The BM1387 is available for around $1-2 each in low quantities from Aliexpress

## Current Status
- I have built 2 of these.
- I can't get any response over serial from the BM1387s yet.
- The current comsumption does change like I would expect when I send a serial command to change the frequency.

## Hardware
- [BM1387B from random AliExpress seller](https://www.aliexpress.com/item/2251832867687077.html). Do these work?? Who knows!
- [40x40mm heatsink and 5V fan](https://www.aliexpress.com/item/2251832861666365.html) from a random AliExpress seller. At least half of these arrived broken in some way. But they are cheap and the working ones do keep the BM1387's nice and cool when used with some thermal compound.
- The serial port is 1.8V. The [FTDI TTL-232RG-VREG1V8-WE](https://www.digikey.com/en/products/detail/ftdi,-future-technology-devices-international-ltd/TTL-232RG-VREG1V8-WE/2441359) USB adapter does the trick, although it's pretty expensive.
- [KiCad 6](https://www.kicad.org) design files
- All of the parts on the board are listed in the KiCad BOM

## Software
Check my [BM1387 Scripts](https://github.com/skot/bm1387_scripts)!

## Connections
![](doc/render.png)

| Pin Name     | Description |
| ----------- | ----------- |
| VIN      | ASIC VIN - powers both ASICs in series. Should be around 1.6V for 0.8V at each ASIC. Will need at least 3A, maybe more! This needs more experimentation.       |
| +5V   | 5V input gets regulated to 1.8V and 0.8V for the BM1387. Also 5V for Fan output. Not much current required.        |
| +5V (Fan)   | 5V output for fan        |
| SPD (Fan)   | Speed input from fan. Doesn't currently go anywhere        |
| 1V8   | 1.8V output. regulated from 5V. Use as a IO reference, or leave unconnected.       |
| RXI   | 1.8V Serial input        |
| TXO   | 1.8V Serial output        |
| RST   | Inverted reset pin to the first BM1387. Unsure what to use this for.        |
| GND   | Common ground        |

## Goals
- Figure out the BM1387 communication and put all of that in an onboard microcontroller like the ESP32-C3 (RISC-V, yum).
- Connect up the BM1387 internal temperature to the onboard microcontroller.
- Connect up the fan speed sensor to the onboard microcontroller.
- Develop some software in Rust to getblocktemplate from a bitcoin node and communicate work to a fleet of bitaxes.
- add an onboard power sensor