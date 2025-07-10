# Appendix F: Volume Knob Patch

A pre-compiled UF2 file to use the Knob as a volume controller is included in the firmwares directory of the Github repository.

If you'd like to build your own, here's a patch you can apply to the code to get you started:

    diff --git a/keyboards/ploopyco/knob/config.h b/keyboards/ploopyco/knob/config.h
    index 9b7d9f5c1a..92d97584cc 100644
    --- a/keyboards/ploopyco/knob/config.h
    +++ b/keyboards/ploopyco/knob/config.h
    @@ -16,7 +16,7 @@

     #pragma once

    -#define POINTING_DEVICE_HIRES_SCROLL_ENABLE 0
    +// #define POINTING_DEVICE_HIRES_SCROLL_ENABLE 0
     #define POINTING_DEVICE_AS5600_ENABLE true

     #define I2C_DRIVER I2CD1
    diff --git a/keyboards/ploopyco/ploopyco.c b/keyboards/ploopyco/ploopyco.c
    index 72fad42aba..24428a73c3 100644
    --- a/keyboards/ploopyco/ploopyco.c
    +++ b/keyboards/ploopyco/ploopyco.c
    @@ -180,23 +180,12 @@ report_mouse_t pointing_device_task_kb(report_mouse_t mouse_report) {
             delta += 4096;
         }

    -    if (detected_host_os() == OS_WINDOWS || detected_host_os() == OS_LINUX) {
    -        // Establish a deadzone to prevent spurious inputs
    -        if (delta > POINTING_DEVICE_AS5600_DEADZONE || delta < -POINTING_DEVICE_AS5600_DEADZONE) {
    -            current_position = ra;
    -            mouse_report.v = delta / POINTING_DEVICE_AS5600_SPEED_DIV;
    -        }
    -    } else {
    -        // Certain operating systems, like MacOS, don't play well with the
    -        // high-res scrolling implementation. For more details, see:
    -        // https://github.com/qmk/qmk_firmware/issues/17585#issuecomment-2325248167
    -        if (delta >= POINTING_DEVICE_AS5600_TICK_COUNT) {
    -            current_position = ra;
    -            mouse_report.v = 1;
    -        } else if (delta <= -POINTING_DEVICE_AS5600_TICK_COUNT) {
    -            current_position = ra;
    -            mouse_report.v = -1;
    -        }
    +    if (delta >= POINTING_DEVICE_AS5600_TICK_COUNT) {
    +        current_position = ra;
    +        tap_code(KC_VOLD);
    +    } else if (delta <= -POINTING_DEVICE_AS5600_TICK_COUNT) {
    +        current_position = ra;
    +        tap_code(KC_VOLU);
         }
     #endif

Happy coding!