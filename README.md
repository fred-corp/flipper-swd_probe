# ARM SWD (Single Wire Debug) Probe

Modern microcontrollers have support for the two wire debug interface SWD, which makes wiring a lot simpler.
When reverse engineering, finding these two pins is a los easier than with JTAG, where you had to wire up twice or more pins. However, finding  the two pins is still a bit of work, which gets simplified even more with this application.

This application tries to detect a valid SWD response on the wires you have picked and beeps when you have found the correct pins, showing the detected ID register and, more important, the SWD pinout. It doesn't matter which two pins you choose, just pick any two from the GPIOs on the breakout header.

To achieve this, the application sends packets and scans the response on all pins and elaborates the pins within a few retries. Using some kind of bisect pattern reduces this number to a hand full of tries, yielding in a seemingly instant detection.

For the user it is as simple as a continuity tester - wire up your two test needles (or accupuncture needles), connect the obvious GND pin and probe all test pads.
Now it depends on your bisect capabilities finding all pad combinations, how long it will take this time.

## Script macros

You can use the following script macros in a `.swd` file to control the SWD probe application:

```txt
# <comment>                                     Comment: lines starting with # are ignored

.label <id>                                     Define a label for jumps

goto <id>                                       Jump to a label

call <script>                                   Call another script file 

status <0/1>                                    Enable/disable status messages

errors <ignore/fail>                            Set error handling mode

beep <0/1>                                      Make a beep on success/failure

leds  <0/1/2> <0/1>                             Select color (0=r, 1=g, 2=b) and state (0=off, 1=on)

delay <ms>                                      Wait for <ms> milliseconds

message <ms> <string> dialog                    Show a message for <ms> milliseconds, add "dialog" to show

max_tries <retries>                             Set maximum retries on read errors, 100ms delay between tries

swd_clock_delay <us>                            Set SWD clock delay in microseconds

swd_idle_bits                                   ? Set number of idle bits between SWD packets

block_size <size>                               Set memory dump block size (4-4096 bytes). HINT: some targets only support 4, else mem_dump writes zeroes only

abort                                           Perform a SWD abort

mem_fill                                        ! Not implemented yet

mem_dump <file> <start> <length> [<flags>]      Dump memory into file, flags&1: skip failed blocks, flags&2: successfully finish even when block failed

mem_ldmst <address> <data> <mask>               Read from <address>, write (old_data & <mask>) | <data>  

mem_write <address> <data>                      Write <data> to <address>

mem_read <address>                              Read from <address>

dp_write                                        ? Write to a Debug Port register

dp_read                                         ? Read from a Debug Port register                   

ap_scan                                         Perform an AP scan

ap_select <ap_id>                               Select a specific AP

ap_read                                         ? Read from an Access Port register

ap_write                                        ? Write to an Access Port register

core_halt                                       ? Halt the core

core_step                                       ? Step the core

core_continue                                   ? Continue core execution

core_regs                                       ? Dump all core registers

core_reg_get <reg>                              ? Get a specific core register

core_reg_set <reg> <data>                       ? Set a specific core register

core_cpuid                                      ? Read the CPUID register
```

> Note: Commands marked with "!" are not implemented yet, commands marked with "?" were not documented on the original source, and were reverse engineered. More details about these commands will be added in future updates.

## Videos

[Video 1](https://cdn.discordapp.com/attachments/954430078882816021/1071603366741938176/20230205_022641.mp4)

[Video 2](https://github.com/xMasterX/all-the-plugins/tree/dev/base_pack/swd_probe)

[Discussion thread](https://discord.com/channels/740930220399525928/1071712925171056690)

> Links might be outdated, please check the latest links in the Discord channel above.
