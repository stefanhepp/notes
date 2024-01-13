ISP Programmer Notes
====================

Pololu ISP Programmer
---------------------

Use Pololu tools (pavr2cmd, pavr2gui) to setup power supply and speed.

Emulates STK500 programmer.

Use avrdude to flash AVR:

```
avrdude -c stk500 -P /dev/ttyACM0 -p m644p
```

avrdude
-------

Configure default programmer: in `~/.avrduderc`, use
```
default_programmer = "stk500";
default_serial = "/dev/ttyACM0";
```


BusPirate v4
------------

TDB
