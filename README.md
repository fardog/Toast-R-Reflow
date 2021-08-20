# Toast-R-Reflow

This is a fork of [nsayer/Toast-R-Reflow][fork] but:

* Only the Controller II is supported
* Firmware builds with [PlatformIO][]
* Eagle files converted to KiCAD (untested, unfixed, and definitely missing
  things)

[fork]: https://github.com/nsayer/Toast-R-Reflow
[PlatformIO]: https://platformio.org/

I take credit for none of this; it's just more convenient for me to make changes
to the firmware this way, and the only reason I converted the hardware to KiCAD
is because that's what I had available to view them.

Included here is what it'll take to get firmware building and uploading using
this repo. The [original README][] has been moved to `README-original.md`.

Just don't blame me if you donk your board; I don't know what I'm doing.

[original README]: ./README-original.md

## Preparing for custom firmware

It's easiest to use PlatformIO once the board can be detected as an Arduino, so
you'll need to burn a bootloader.

What's required to do so isn't outlined in detail here, but here's what you'll
need to know:

* The ICSP header is the blue header on the back of the unit; listed as `JP4`,
  near the open-source hardware logo
* Its pinout is the usual ICSP layout; left-to-right, top-to-bottom:
  * Reset
  * SCK
  * MISO
  * GND
  * MOSI
  * VCC (5v)
* It runs an Atmega328PB, and is 5v tolerant
* I used an Arduino Micro as an ISP without issue, with reset wired to digital
  pin 10

The original README says that the board would work fine if you pretend it's an
Arduino UNO; that just didn't work for me given that board is an Atmega328P, and
this is an Atmega328PB. I read they're maybe mostly compatible but without
finding anything difinitive there, I got it working as the PB model.

For a bootloader that supports the Atmega328PB, use [MiniCore][]. Follow its
instructions to burn the loader; I used settings:

* Board: MiniCore -> Atmega328
* Clock: External 16 MHz
* BOD: BOD2.7V
* EEPROM: EEPROM retained
* Compiler LTO: LTO disabled
* Variant: 328PB
* Bootloader: Yes (UART0)

[MiniCore]: https://github.com/MCUdude/MiniCore

## FTDI Header

The board has a 6-pin inline header labeled "FTDI" which can be used for
programming, but you need to solder the jumper pins (labeled `SJ1`) to activate
the reset line. In my case, it was also missing a 0.1uF capacitor `C10` on the
line, but I have to assume that's not a common error.

## Building Custom Firmware

The `platformio.ini` in this repo is set up correctly; building and uploading
the firmware should work without additional settings.

## Changes to the firmware

* `SiO2` profile for drying silica gel; 110°C for 3hr
