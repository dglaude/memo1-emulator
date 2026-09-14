# Fork goal

I want to emulate the Memo-1 computer by @MemoireMorte at https://github.com/MemoireMorte/Memo-1 on some physical hardware.

There is a emulator that run on PC by @Bipcollector at https://github.com/Bipcollector/Memu-1 but it is all virtual.

So the plan is to run that on the FruitJam from @Adafruit that permit the use of an HDMI screen and a USB keyboard.

Because I don't want to write too much code and not re-invent the wheel, I wanted to check for existing codebase I could use.

### Candidate Emulator Bases

Here are 3 emulator that emulate retro computer based on 6502 (and other) and could be a good sofware based for Memo-1 that is similar.

All the low level support for Adafruit Fruit Jam is already written by other, I just want to focus on Memo-1 singularity.

**1. Fruit Jam Reload (Apple //e)**

* **GitHub Repository**: [adafruit/reload-emulator](https://www.google.com/search?q=https://github.com/adafruit/reload-emulator) (Upstream: [vsladkov/reload-emulator](https://github.com/vsladkov/reload-emulator)
* **Adafruit Learn Guide**: [Apple //e Emulator on Fruit Jam](https://learn.adafruit.com/apple-e-emulator-on-fruit-jam)
* **Original Author**: Vladimir Sladkov (`vsladkov`)
* **Fruit Jam Adaptation Author**: Tim C (`FoamyGuy`)

**2. Adafruit MCUME**

* **GitHub Repository**: [adafruit/MCUME](https://www.google.com/search?q=https://github.com/adafruit/MCUME) (Upstream: [Jean-MarcHarvengt/MCUME](https://github.com/Jean-MarcHarvengt/MCUME))
* **Adafruit Learn Guide**: [MCUME Emulators on Fruit Jam](https://learn.adafruit.com/mcume-emulators-on-fruit-jam)[cite: 4]
* **Original Author**: Jean-Marc Harvengt (`Jean-MarcHarvengt`)
* **Fruit Jam Adaptation Author**: Adafruit (Tim C[cite: 4] and Jeff Epler)

**3. retroJam Multi-Emulator**

* **GitHub Repository**: [PicoPlus-devel/retroJam](https://github.com/PicoPlus-devel/retroJam)
* **Adafruit Learn Guide**: None (Third-party community project)
* **Original Author**: Frank Hoedemakers (`PicoPlus-devel`)
* **Fruit Jam Adaptation Author**: Frank Hoedemakers (`PicoPlus-devel`)

### Selection Rational: Why Fruit Jam Reload Was Chosen

**Fruit Jam Reload** provides the closest architectural alignment to the Memo-1 while keeping software complexity to a minimum:

* **CPU Family Parity**: It includes a cycle-stepped 6502 core running at 1 MHz[cite: 1], identical to the clock speed and instruction set needed to execute the Memo-1 ROM without modification[cite: 1].
* **Text-Oriented Display Architecture**: Unlike console emulators that rely heavily on tilemaps and sprite hardware, the Apple //e base natively handles a 40-column text mode[cite: 1]. This maps cleanly to the Memo-1’s 40×24 Videotex/Minitel screen format.
* **Direct Physical Keyboard Pipeline**: Designed specifically for personal computers where users enter text and write BASIC programs[cite: 1], the project routes USB HID keystrokes straight into system characters[cite: 1, 2] rather than downsampling them into gamepad button presses.
* **Matching Sound Primitive**: The Apple //e generates sound via a 1-bit speaker toggle[cite: 2], which directly mirrors how the Memo-1 uses bit 7 of its VIA 65C22 to trigger audio tones. The audio pathway to the Fruit Jam’s onboard TLV320 DAC is already implemented and validated[cite: 2].
* **Lean Codebase**: With over 90% written in straightforward C[cite: 1], the repository avoids multi-system framework abstractions, making it simple to strip away the Apple-specific MMU/disk controller[cite: 1] and attach the Memo-1 memory map (RAM, ROM, VIA 65C22, and ACIA 6551).

---

# Fruit Jam Reload - Portable Cycle-Stepped Emulator for Retro Computers

Emulated system:

### Memo-1 ( Target, currently not even a WIB)

### Apple //e
  - 128 KB RAM installed 
  - Extended 80 column card in the AUX slot
  - Disk II controller and 1 drive in slot 6
  - ProDOS hard disk controller in slot 7

## Quickstart

Grab a UF2 file from the releases page and the Total Replay image from archive.org.

Put `Total Replay v5.2.hdv` (use exactly that filename) in the top directory of an SD card.

Copy the Reload Emulator UF2 file to your Fruit Jam's RP2350 drive.

Plug in a keyboard & gamepad. Insert the SD card. Connect to a compatible display.

Turn it on & play!

## Requirements & Building from source

Refer to the github actions files for the steps to build reload-emulator.
