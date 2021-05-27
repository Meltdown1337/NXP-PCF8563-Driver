# NXP-PCF8563-Driver
This class provides a hardware driver for the NXP PCF8563 real time clock (RTC). This RTC and calendar is optimized for low power consumption. It communicates with the host imp via I²C at a speed of up to 400Kbps.

### Class Usage

The constructor takes two required parameters: a configured imp I&up2;c bus and the PCF8563’s 8-bit I²C address (default: 0xA2)

The third parameter is optional: pass in ``true`` to get extra debugging information in the log (default: ``false``).

``hardware.i2c89.configure(CLOCK_SPEED_400_KHZ);``

`` ``

``rtc <- PCF8563(hardware.i2c89, 0xA2, true);``
