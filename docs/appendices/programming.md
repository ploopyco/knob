# Appendix D: QMK Firmware Programming


## Working with QMK

Ploopy devices are fairly easy to program and reprogram, thanks to the excellent work by all of the developers and maintainers of the [QMK firmware suite](https://github.com/qmk/qmk_firmware).


## Before you begin

If you have never used QMK before, go through [all of the steps in the QMK guide to set up your environment](https://docs.qmk.fm/#/newbs_getting_started).

QMK was built for keyboards, so you'll see lots of references to code that looks like the following:

`-kb <keyboard>`

Whenever you see that, use the following syntax:

`-kb ploopyco/knob`

and you'll be fine.


## Building the Ploopy device firmware

Using QMK MSYS or your compilation toolchain of choice, compile the firmware with the following command:

`qmk compile -kb ploopyco/knob/rev1_001 -km default`

For more details on building QMK firmware in general, see the [QMK firmware guide](https://docs.qmk.fm/#/newbs_building_firmware).


## Putting the Ploopy device into bootloader mode

Putting the Ploopy device into bootloader mode is very simple and straightforward.

1. Unplug it from your computer.
2. Open it by removing the screws in the Base and removing the Top.
3. Look for a pair of vias (gold-plated holes) on the board ([here's a photo](https://ploopy.co/wp-content/uploads/2025/06/knob-vias.jpg)).
4. Get a paper clip (non-insulated, i.e. no plastic shit covering it) or a pair of tweezers, or some wire. Whatever you've got on hand that's metal.
5. Stick the paper clip or tweezers into the holes. You're trying to form an electrical connection between the two holes.
6. While you've got the two vias connected with your metal bridge, plug the Ploopy device into your computer.
7. The computer should recognise that a mass storage device was just plugged in. Once this is done, you should be able to drag and drop files onto the Ploopy device, as if the board was a USB drive. Feel free to remove the tweezers or paperclip at this point.

And that's it. While plugged in this way, the Ploopy device will accept new firmware.

If you want to upload a new firmware file (a ".uf2" file, like "ploopy_knob_v42069" or something), just drag it into the folder, and it'll automatically install on the Ploopy device and restart itself.

Whenever you want to put new firmware onto the Ploopy device, go through these steps again.

**TIP**: If your firmware is in some kind of strange state and uploading new firmware isn't fixing it, try uploading [a flash nuke](https://learn.adafruit.com/getting-started-with-raspberry-pi-pico-circuitpython/circuitpython#flash-resetting-uf2-3083182) to the knob before flashing the new firmware. It wipes the memory of the knob completely clean, which can help clear a few types of errors.


## And that's it!

Happy customizing!
