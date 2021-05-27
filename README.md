# NXP-PCF8563-Driver
This class provides a hardware driver for the NXP PCF8563 real time clock (RTC). This RTC and calendar is optimized for low power consumption. It communicates with the host imp via I²C at a speed of up to 400Kbps.

### Class Usage

The constructor takes two required parameters: a configured imp I&up2;c bus and the PCF8563’s 8-bit I²C address (default: 0xA2)

The third parameter is optional: pass in ``true`` to get extra debugging information in the log (default: ``false``).

```
hardware.i2c89.configure(CLOCK_SPEED_400_KHZ);
rtc <- PCF8563(hardware.i2c89, 0xA2, true);
```
# Class Drivers

### sync()

This method sets the PCF8563 to the date and time provided by the imp’s own RTC, which is itself synchronized with the impCloud™ when the device connects.

```
// Sync the PCF8563 and imp RTC
rtc.sync();
```

### getDateAndTime()

Note that the PCF8563 stores months in the calendar fashion (ie. January = 1, Febraury = 2, etc). Because Squirrel’s date() function zero-indexes month values (ie. January = 0, February = 1, etc) the PCF8563 class does the same and modifies the stored/retrieved value automatically.

Additionally, the PCF8563 stores yeas in the form 00-99 whereas date() returns the actual year (eg. 2016). Again, the class adapts the hardware value to match the Squirrel function.

### setDateAndTime(day, month, year, wday, hour, min, sec)

This method can be used to set the PCF8563 explicitly, should you make use of an alternative source to the imp’s own RTC, or to set the PCF8563 to begin timing from an arbitrary date and time. Again, the parameters match those used by Squirrel’s date() function.

### checkClock()

This method returns a boolean value indicating whether or not the clock’s integrity has been maintained. The value is set by reading the PCF8563’s low-voltage register, which is set when its VDD pin falls below a critical value. This can be used in cases where the RTC is backed up by a cell battery. If host imp powers down, this function can be checked on waking to issue a warning that the back-up battery needs replacing. Under normal imp operating, when the PCF8563 is not running off battery, the low-voltage register will not be set.

The low-voltage register remains set until manually cleared, which can be achieved by calling *clearClockCheck().*

```
local i = rtc.checkClock();
local s = "Clock integrity " + (i ? "good" : "bad");
server.log(s);

if (!i) {
    // Reset clock low-voltage register after warning
    rtc.clearClockCheck();
}
```
