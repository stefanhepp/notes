Windows Tipps and Tricks
========================

Sleep Mode and Power Saving
---------------------------

### Find out what is preventing standby
- Run in Administrator commandline
  ```
  powercfg -requests
  powercfg –devicequery wake_armed
  ```
- Check what caused the last wakeup
  ```
  powercfg -lastwake
  ```

