# Appendix E: MacOS Compatibility

Currently, MacOS does its own smoothing to implement high-resolution scrolling. As a result of that, HID-based high-res scrolling (such as the kind implemented in QMK) doesn't work well. More discussion on this topic [can be seen here](https://github.com/qmk/qmk_firmware/issues/17585).

The current firmware implements notched scrolling for MacOS, which then smoothes the motion. However, it isn't as precise as it is in the Windows and Linux implementations. For this reason, we designate that high-resolution scrolling is not officially supported by the Knob.


## Workaround

A possible workaround is to utilise [the open-source library "discrete-scroll"](https://github.com/emreyolcu/discrete-scroll). This disables the smooth scrolling on MacOS.[^1]

After that, a new version of the firmware will have to be built. Begin by modifying the QMK file "keyboards/ploopyco/ploopyco.c". Change the following lines:

    #ifdef POINTING_DEVICE_AS5600_ENABLE
        // Get AS5600 rawangle
        uint16_t ra = get_rawangle();
        int16_t delta = (int16_t)(ra - current_position);

        // Wrap into [-2048, 2047] to get shortest direction
        if (delta > 2048) {
            delta -= 4096;
        } else if (delta < -2048) {
            delta += 4096;
        }

        if (d_os == OS_WINDOWS || d_os == OS_LINUX) {
            // Establish a deadzone to prevent spurious inputs
            if (delta > POINTING_DEVICE_AS5600_DEADZONE || delta < -POINTING_DEVICE_AS5600_DEADZONE) {
                current_position = ra;
                mouse_report.v = delta / POINTING_DEVICE_AS5600_SPEED_DIV;
            }
        } else {
            // Certain operating systems, like MacOS, don't play well with the
            // high-res scrolling implementation. For more details, see:
            // https://github.com/qmk/qmk_firmware/issues/17585#issuecomment-2325248167
            if (delta >= POINTING_DEVICE_AS5600_TICK_COUNT) {
                current_position = ra;
                mouse_report.v = 1;
            } else if (delta <= -POINTING_DEVICE_AS5600_TICK_COUNT) {
                current_position = ra;
                mouse_report.v = -1;
            }
        }
    #endif

to this:

    #ifdef POINTING_DEVICE_AS5600_ENABLE
        // Get AS5600 rawangle
        uint16_t ra = get_rawangle();
        int16_t delta = (int16_t)(ra - current_position);

        // Wrap into [-2048, 2047] to get shortest direction
        if (delta > 2048) {
            delta -= 4096;
        } else if (delta < -2048) {
            delta += 4096;
        }

        // Establish a deadzone to prevent spurious inputs
        if (delta > POINTING_DEVICE_AS5600_DEADZONE || delta < -POINTING_DEVICE_AS5600_DEADZONE) {
            current_position = ra;
            mouse_report.v = delta / POINTING_DEVICE_AS5600_SPEED_DIV;
        }
    #endif

After recompiling and reflashing (see [Appendix D: QMK Firmware Programming](programming.md) for more details), you should get smooth scrolling, just the same as on Windows or Linux.


[^1]: Thanks to [asyncmeow](https://fedi.rrr.sh/@pearl) for her contributions to this fix.