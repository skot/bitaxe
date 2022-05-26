# The bitaxe
![](doc/assembled.png)
an experimental BM1387 bitcoin mining machine

## BM1387
![bm1387 pic](doc/bm1387.png)

- The BM1387 is a undocumented SHA256 mining ASIC from Bitmain. It's mostly used in the Antminer S9
- Bitmain claims the BM1387 has 0.098j/Gh efficiency
- The BM1387 is available for around $1-2 each in low quantities from Aliexpress

## Current Status
- I have built 2 of these.
- I can't get any response from the BM1387s yet.
- The current comsumption does change like I would exprect when I send a serial command to change the frequency.

## Hardware
- BM1387B from random AliExpress seller. Do these work?? Who knows!
- [40x40mm heatsink and 5V fan](https://www.aliexpress.com/item/2251832861666365.html) from a random AliExpress seller. At least half of these arrived broken in some way. But they are cheap and the working ones do keep the BM1387's nice and cool when used with some thermal compound.

## Software
Check my [BM1387 Scripts](https://github.com/skot/bm1387_scripts)!

## Connections
![](doc/render.png)

| Pin Name     | Description |
| ----------- | ----------- |
| VIN      | ASIC VIN - powers both ASICs in series. should be around 1.6V       |
| +5V   | 5V input gets regulated to 1.8V and 0.8V for the BM1387. Also 5V for Fan output        |
| +5V (Fan)   | 5V output for fan        |
| SPD (Fan)   | Speed input from fan. Doesn't currently go anywhere        |
| 1V8   | 1.8V output. regulated from 5V. Use as a IO reference        |
| RXI   | 1.8V Serial input        |
| TXO   | 1.8V Serial output        |
| RST   | Inverted reset pin to the first BM1387. Unsure what to use this for.        |
| GND   | Common ground        |