# Python Time Module - Comprehensive Reference

## Overview

The `time` module provides various functions for working with time in Python. This module is part of the Python Standard Library and is always available. It handles time-related operations such as getting the current time, measuring time intervals, formatting time, and working with different time representations.

## Important Concepts

### The Epoch
The epoch is the starting point for measuring time. On all platforms, the epoch is January 1, 1970, 00:00:00 (UTC). Time is measured as seconds since this epoch.

### Seconds Since the Epoch
This refers to the total number of elapsed seconds since the epoch. Leap seconds are excluded from this total on all POSIX-compliant platforms.

### UTC and Local Time
- **UTC** (Coordinated Universal Time) is the international time standard
- **Local time** is time adjusted for the local timezone and daylight saving time rules

### DST (Daylight Saving Time)
An adjustment of the timezone, usually by one hour, during part of the year. DST rules vary by location and can change over time.

### struct_time Object
Many functions return or accept a `struct_time` object, which is a named tuple containing nine time-related fields.

## Functions

### time.time()
Returns the current time in seconds since the epoch as a floating-point number.

**Syntax:**
```python
time.time() → float
```

**Example:**
```python
import time
current_time = time.time()
print(current_time)  # Output: 1672214933.6804628
```

### time.time_ns()
Similar to `time()` but returns time as an integer number of nanoseconds since the epoch. This provides higher precision.

**Syntax:**
```python
time.time_ns() → int
```

**Example:**
```python
import time
current_time_ns = time.time_ns()
print(current_time_ns)  # Output: 1672214933680462800
```

### time.ctime([secs])
Converts a time expressed in seconds since the epoch to a readable string representing local time.

**Parameters:**
- `secs` (optional): Time in seconds since epoch. If not provided, uses current time.

**Returns:** String in format: `'Sun Jun 20 23:21:05 1993'`

**Example:**
```python
import time
readable_time = time.ctime()
print(readable_time)  # Output: 'Wed Dec 28 08:44:04 2022'

# With specific time
specific_time = time.ctime(1672215379)
print(specific_time)  # Output: 'Wed Dec 28 08:49:39 2022'
```

### time.gmtime([secs])
Converts a time expressed in seconds since the epoch to a `struct_time` in UTC.

**Parameters:**
- `secs` (optional): Time in seconds since epoch. If not provided, uses current time.

**Returns:** `struct_time` object with DST flag always set to zero

**Example:**
```python
import time
utc_time = time.gmtime(1672214933)
print(utc_time)
# Output: time.struct_time(tm_year=2022, tm_mon=12, tm_mday=28, 
#         tm_hour=8, tm_min=8, tm_sec=53, tm_wday=2, tm_yday=362, tm_isdst=0)

# Access individual fields
print(utc_time.tm_year)  # Output: 2022
print(utc_time.tm_hour)  # Output: 8
```

### time.localtime([secs])
Converts a time expressed in seconds since the epoch to a `struct_time` in local time.

**Parameters:**
- `secs` (optional): Time in seconds since epoch. If not provided, uses current time.

**Returns:** `struct_time` object with DST flag set appropriately

**Example:**
```python
import time
local_time = time.localtime()
print(local_time)
# Output: time.struct_time(tm_year=2022, tm_mon=12, tm_mday=28,
#         tm_hour=15, tm_min=25, tm_sec=25, tm_wday=0, tm_yday=148, tm_isdst=1)
```

### time.mktime(t)
Converts a `struct_time` or 9-tuple representing local time to seconds since the epoch. This is the inverse of `localtime()`.

**Parameters:**
- `t`: `struct_time` or 9-tuple expressing time in local time (not UTC)

**Returns:** Floating-point number (seconds since epoch)

**Example:**
```python
import time
time_tuple = (2022, 12, 28, 8, 44, 4, 4, 362, 0)
seconds = time.mktime(time_tuple)
print(seconds)  # Output: 1672215844.0
```

### time.asctime([t])
Converts a tuple or `struct_time` to a readable string.

**Parameters:**
- `t` (optional): `struct_time` or tuple. If not provided, uses current local time.

**Returns:** String in format: `'Sun Jun 20 23:21:05 1993'`

**Example:**
```python
import time
t = (2022, 12, 28, 8, 44, 4, 4, 362, 0)
result = time.asctime(t)
print(result)  # Output: 'Fri Dec 28 08:44:04 2022'
```

### time.strftime(format[, t])
Formats a `struct_time` or tuple as a string according to a format specification.

**Parameters:**
- `format`: String with format directives
- `t` (optional): `struct_time` or tuple. If not provided, uses current local time.

**Returns:** Formatted string

**Common Format Directives:**
- `%Y` - Year with century (e.g., 2022)
- `%m` - Month as decimal [01-12]
- `%d` - Day of month [01-31]
- `%H` - Hour (24-hour clock) [00-23]
- `%M` - Minute [00-59]
- `%S` - Second [00-61]
- `%A` - Full weekday name
- `%a` - Abbreviated weekday name
- `%B` - Full month name
- `%b` - Abbreviated month name
- `%p` - AM or PM
- `%I` - Hour (12-hour clock) [01-12]
- `%z` - UTC offset (+HHMM or -HHMM)
- `%Z` - Timezone name
- `%%` - Literal '%' character

**Example:**
```python
import time
named_tuple = time.localtime()
time_string = time.strftime("%m/%d/%Y, %H:%M:%S", named_tuple)
print(time_string)  # Output: '12/28/2022, 15:25:25'

# Another format
time_string2 = time.strftime("%A, %B %d, %Y")
print(time_string2)  # Output: 'Wednesday, December 28, 2022'
```

### time.strptime(string[, format])
Parses a string representing time according to a format specification. This is the inverse of `strftime()`.

**Parameters:**
- `string`: String to parse
- `format` (optional): Format specification. Defaults to `"%a %b %d %H:%M:%S %Y"`

**Returns:** `struct_time` object

**Example:**
```python
import time
time_string = "14 July, 2023"
result = time.strptime(time_string, "%d %B, %Y")
print(result)
# Output: time.struct_time(tm_year=2023, tm_mon=7, tm_mday=14, 
#         tm_hour=0, tm_min=0, tm_sec=0, tm_wday=4, tm_yday=195, tm_isdst=-1)

# Another example
time_string2 = "30 Nov 00"
result2 = time.strptime(time_string2, "%d %b %y")
print(result2)
# Output: time.struct_time(tm_year=2000, tm_mon=11, tm_mday=30, ...)
```

### time.sleep(secs)
Suspends execution of the current thread for the specified number of seconds.

**Parameters:**
- `secs`: Number of seconds to sleep (can be a floating-point number)

**Returns:** None

**Example:**
```python
import time
print("Starting...")
time.sleep(2.5)  # Pause for 2.5 seconds
print("Finished after 2.5 seconds")
```

### time.monotonic()
Returns the value of a monotonic clock in fractional seconds. A monotonic clock cannot go backwards and is not affected by system clock updates. Only the difference between two calls is meaningful.

**Syntax:**
```python
time.monotonic() → float
```

**Example:**
```python
import time
start = time.monotonic()
# Do some work
time.sleep(1)
end = time.monotonic()
elapsed = end - start
print(f"Elapsed time: {elapsed} seconds")  # Output: ~1.0 seconds
```

### time.monotonic_ns()
Similar to `monotonic()` but returns time as nanoseconds (integer).

**Syntax:**
```python
time.monotonic_ns() → int
```

**Example:**
```python
import time
start = time.monotonic_ns()
time.sleep(0.001)
end = time.monotonic_ns()
print(f"Elapsed: {end - start} nanoseconds")
```

### time.perf_counter()
Returns the value of a performance counter with the highest available resolution. This is the best clock for measuring short durations. It includes time elapsed during sleep.

**Syntax:**
```python
time.perf_counter() → float
```

**Example:**
```python
import time
start = time.perf_counter()
# Code to measure
for i in range(1000000):
    pass
end = time.perf_counter()
print(f"Execution time: {end - start} seconds")
```

### time.perf_counter_ns()
Similar to `perf_counter()` but returns time as nanoseconds (integer).

**Syntax:**
```python
time.perf_counter_ns() → int
```

### time.process_time()
Returns the sum of system and user CPU time of the current process in fractional seconds. It does not include time elapsed during sleep.

**Syntax:**
```python
time.process_time() → float
```

**Example:**
```python
import time
start = time.process_time()
# CPU-intensive task
sum([i**2 for i in range(1000000)])
end = time.process_time()
print(f"CPU time used: {end - start} seconds")
```

### time.process_time_ns()
Similar to `process_time()` but returns time as nanoseconds (integer).

**Syntax:**
```python
time.process_time_ns() → int
```

### time.thread_time()
Returns the sum of system and user CPU time of the current thread in fractional seconds. It does not include time elapsed during sleep. Available on Linux, Unix, and Windows.

**Syntax:**
```python
time.thread_time() → float
```

**Example:**
```python
import time
start = time.thread_time()
# Thread work
sum(range(1000000))
end = time.thread_time()
print(f"Thread CPU time: {end - start} seconds")
```

### time.thread_time_ns()
Similar to `thread_time()` but returns time as nanoseconds (integer).

**Syntax:**
```python
time.thread_time_ns() → int
```

### time.get_clock_info(name)
Gets information about the specified clock as a namespace object.

**Parameters:**
- `name`: Clock name (string). Valid values:
  - `'monotonic'`
  - `'perf_counter'`
  - `'process_time'`
  - `'thread_time'`
  - `'time'`

**Returns:** Namespace object with attributes:
- `adjustable`: Boolean indicating if clock can jump forward or backward
- `implementation`: Name of underlying C function
- `monotonic`: Boolean indicating if clock cannot go backward
- `resolution`: Clock resolution in seconds (float)

**Example:**
```python
import time
info = time.get_clock_info('monotonic')
print(f"Resolution: {info.resolution}")
print(f"Monotonic: {info.monotonic}")
print(f"Adjustable: {info.adjustable}")
```

### time.clock_getres(clk_id)
Returns the resolution (precision) of the specified clock. Available on Unix systems only.

**Parameters:**
- `clk_id`: Clock ID constant

**Returns:** Float (resolution in seconds)

**Example:**
```python
import time
resolution = time.clock_getres(time.CLOCK_MONOTONIC)
print(f"Clock resolution: {resolution} seconds")
```

### time.clock_gettime(clk_id)
Returns the time of the specified clock. Available on Unix systems only.

**Parameters:**
- `clk_id`: Clock ID constant

**Returns:** Float (time in seconds)

**Example:**
```python
import time
current = time.clock_gettime(time.CLOCK_MONOTONIC)
print(f"Monotonic clock time: {current}")
```

### time.clock_gettime_ns(clk_id)
Similar to `clock_gettime()` but returns time as nanoseconds (integer).

**Syntax:**
```python
time.clock_gettime_ns(clk_id) → int
```

### time.clock_settime(clk_id, time)
Sets the time of the specified clock. Currently only `CLOCK_REALTIME` is accepted. Requires appropriate privileges. Available on Unix systems (not Android, not iOS).

**Parameters:**
- `clk_id`: Clock ID (currently only `CLOCK_REALTIME`)
- `time`: Time in seconds (float)

**Example:**
```python
import time
# Requires root/admin privileges
time.clock_settime(time.CLOCK_REALTIME, 1672214933.5)
```

### time.clock_settime_ns(clk_id, time)
Similar to `clock_settime()` but sets time with nanoseconds (integer).

**Syntax:**
```python
time.clock_settime_ns(clk_id, time: int)
```

### time.pthread_getcpuclockid(thread_id)
Returns the clock ID of the thread-specific CPU-time clock for the specified thread. Available on Unix systems only.

**Parameters:**
- `thread_id`: Thread ID from `threading.get_ident()` or `threading.Thread.ident`

**Returns:** Clock ID

**Example:**
```python
import time
import threading
thread_id = threading.get_ident()
clk_id = time.pthread_getcpuclockid(thread_id)
```

### time.tzset()
Resets the time conversion rules used by the library routines based on the `TZ` environment variable. Available on Unix systems only.

**Example:**
```python
import time
import os
os.environ['TZ'] = 'US/Eastern'
time.tzset()
print(time.tzname)  # Output: ('EST', 'EDT')
```

## Class: struct_time

The `struct_time` class represents time as a named tuple with nine fields. Values can be accessed by index or attribute name.

**Attributes:**

| Index | Attribute | Description | Range |
|-------|-----------|-------------|-------|
| 0 | tm_year | Year | (e.g., 2022) |
| 1 | tm_mon | Month | [1, 12] |
| 2 | tm_mday | Day of month | [1, 31] |
| 3 | tm_hour | Hour | [0, 23] |
| 4 | tm_min | Minute | [0, 59] |
| 5 | tm_sec | Second | [0, 61] |
| 6 | tm_wday | Weekday | [0, 6] (Monday is 0) |
| 7 | tm_yday | Day of year | [1, 366] |
| 8 | tm_isdst | Daylight saving flag | 0, 1, or -1 |
| N/A | tm_zone | Timezone abbreviation | (string) |
| N/A | tm_gmtoff | Offset east of UTC | (seconds) |

**DST Flag Values:**
- `0`: DST is not in effect
- `1`: DST is in effect
- `-1`: DST information is not available

**Example:**
```python
import time
t = time.localtime()
print(t.tm_year)   # Access by attribute
print(t[0])        # Access by index (same as tm_year)
print(t.tm_zone)   # Timezone abbreviation
print(t.tm_gmtoff) # Offset from UTC
```

## Clock ID Constants

These constants are used with clock functions on Unix systems:

### time.CLOCK_REALTIME
System-wide real-time clock. Can be set (requires privileges). Used with `clock_settime()`.

### time.CLOCK_MONOTONIC
Clock that cannot be set and represents monotonic time since an unspecified starting point.

### time.CLOCK_MONOTONIC_RAW
Similar to `CLOCK_MONOTONIC` but not subject to NTP adjustments. Available on Linux and macOS.

### time.CLOCK_MONOTONIC_RAW_APPROX
Similar to `CLOCK_MONOTONIC_RAW` but cached at context switches (less accurate). Available on macOS.

### time.CLOCK_BOOTTIME
Identical to `CLOCK_MONOTONIC` but includes time when system is suspended. Available on Linux.

### time.CLOCK_PROCESS_CPUTIME_ID
High-resolution per-process timer from the CPU.

### time.CLOCK_THREAD_CPUTIME_ID
Thread-specific CPU-time clock.

### time.CLOCK_UPTIME
Time since system boot, excluding time when suspended. Available on FreeBSD and OpenBSD.

### time.CLOCK_UPTIME_RAW
Monotonic clock unaffected by frequency adjustments and not incremented during sleep. Available on macOS.

### time.CLOCK_UPTIME_RAW_APPROX
Similar to `CLOCK_UPTIME_RAW` but cached (less accurate). Available on macOS.

### time.CLOCK_HIGHRES
Solaris-specific high-resolution timer with nanosecond resolution.

### time.CLOCK_PROF
High-resolution per-process CPU timer. Available on FreeBSD, NetBSD, and OpenBSD.

### time.CLOCK_TAI
International Atomic Time. Requires current leap second table. Available on Linux.

## Timezone Constants

These constants provide timezone information. Note that values are determined at module load time or when `tzset()` is called.

### time.timezone
Offset of local non-DST timezone in seconds west of UTC. Negative in Western Europe, positive in the US, zero in the UK.

**Example:**
```python
import time
print(time.timezone)  # Output varies by location (e.g., 18000 for US Eastern)
```

### time.altzone
Offset of local DST timezone in seconds west of UTC. Only meaningful if `daylight` is nonzero.

**Example:**
```python
import time
if time.daylight:
    print(time.altzone)
```

### time.daylight
Nonzero if a DST timezone is defined for the locale.

**Example:**
```python
import time
print(time.daylight)  # Output: 1 if DST is defined, 0 otherwise
```

### time.tzname
Tuple of two strings: local non-DST timezone name and local DST timezone name.

**Example:**
```python
import time
print(time.tzname)  # Output: ('EST', 'EDT') on US Eastern
```

## Time Conversion Table

This table shows how to convert between different time representations:

| From | To | Function |
|------|-----|----------|
| Seconds since epoch | struct_time (UTC) | `gmtime()` |
| Seconds since epoch | struct_time (local) | `localtime()` |
| struct_time (UTC) | Seconds since epoch | `calendar.timegm()` |
| struct_time (local) | Seconds since epoch | `mktime()` |
| struct_time | String | `strftime()` or `asctime()` |
| String | struct_time | `strptime()` |
| Seconds since epoch | String | `ctime()` |

## Practical Examples

### Measuring Execution Time
```python
import time

# Method 1: Using perf_counter for precise timing
start = time.perf_counter()
# Code to measure
sum(range(1000000))
end = time.perf_counter()
print(f"Execution time: {end - start:.6f} seconds")

# Method 2: Using time for absolute timestamps
start = time.time()
time.sleep(1)
end = time.time()
print(f"Elapsed: {end - start:.2f} seconds")
```

### Formatting Current Date and Time
```python
import time

# Get current time
current = time.localtime()

# Different formats
print(time.strftime("%Y-%m-%d %H:%M:%S", current))
# Output: 2022-12-28 15:25:25

print(time.strftime("%A, %B %d, %Y", current))
# Output: Wednesday, December 28, 2022

print(time.strftime("%I:%M %p", current))
# Output: 03:25 PM
```

### Parsing Date Strings
```python
import time

# Parse various date formats
date1 = time.strptime("2023-07-14", "%Y-%m-%d")
date2 = time.strptime("14/07/2023", "%d/%m/%Y")
date3 = time.strptime("July 14, 2023", "%B %d, %Y")

print(date1.tm_year, date1.tm_mon, date1.tm_mday)
# Output: 2023 7 14
```

### Creating Delays
```python
import time

# Simple delay
print("Starting...")
time.sleep(2)
print("2 seconds later")

# Sub-second delay
time.sleep(0.5)
print("0.5 seconds later")

# Using in loops
for i in range(5):
    print(f"Count: {i}")
    time.sleep(1)
```

### Working with Timezones
```python
import time

# Get current time in different representations
utc = time.gmtime()
local = time.localtime()

print(f"UTC: {time.strftime('%Y-%m-%d %H:%M:%S', utc)}")
print(f"Local: {time.strftime('%Y-%m-%d %H:%M:%S', local)}")
print(f"Timezone: {local.tm_zone}")
print(f"UTC offset: {local.tm_gmtoff / 3600} hours")
```

## Important Notes

1. **Precision**: While functions return floating-point numbers, actual precision depends on the platform and may be less than suggested.

2. **Year 2038 Problem**: On 32-bit systems, dates far in the future (typically after 2038) may not be handled correctly.

3. **Leap Seconds**: Leap seconds are excluded from time calculations on POSIX-compliant platforms.

4. **Monotonic Clocks**: Use `monotonic()` or `perf_counter()` for measuring durations, not `time()`, as system clock changes can affect `time()`.

5. **Sleep Accuracy**: `sleep()` suspends execution for at least the specified time, but may be longer due to system scheduling.

6. **Platform Differences**: Some functions and constants are only available on specific platforms (Unix, Linux, macOS, Windows).

7. **Timezone Data**: For timezone constants, it is recommended to use `tm_gmtoff` and `tm_zone` from `localtime()` results rather than the global constants.

## Related Modules

- **datetime**: More object-oriented interface for dates and times
- **calendar**: Calendar-related functions
- **locale**: Internationalization services affecting time formatting
- **timeit**: Module for measuring execution time of code snippets

---
