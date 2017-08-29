---
title: "Paper Notes: Bootstomp: on the Security of Bootloaders in Mobile Devices"
date: 2017-08-29T23:41:35+03:00
draft: false
---

Title: Bootstomp: on the Security of Bootloaders in Mobile Devices [[PDF]](../../papers/bootstomp.pdf)

BootStomp is a tool that searches for vulnerabilities in bootloaders of mobile
devices. There are two types of vulnerabilities covered.  The first is code
execution in the bootloader via a memory corruption bug. The second is
unlocking the bootloader without the consent of the user which enables code
execution with higher permissions and possibly persistence.

BootStomp uses dynamic symbolic execution to perform taint analysis on
bootloader code typically runnning on ARM's exception level 1 (EL1). Hex-Rays
is used to identify seeds of taint, meaning untusted input from the
non-volatile memory, for example `read_mmc()`. Then Hex-Rays identifies sinks
of taint, or interesting code that is typically dangerous (memcpy-like
functions) or filesystem writes.

Finally symbolic execution (using angr) and taint analysis help identify sinks
of taint that process untrusted input.  BootStomp uses under-constrained
symbolic execution, examining only functions that call input functions that
provide taint. Also, a simple type-inference is performed to cut down on
symbolic variables and concretize as many variables as possible.

To mitigate state explosion, a number of execution strategies are used during symbolic execution.

* If there is no taint, skip any function calls
* If a function does not take tainted input as argument, skip it Use an
* adaptive inter-function level to skip functions that are too far from the
  entry point
* Loops are executed only once

The above can of course lead to situations where taint is not propagated appropriately resulting in false negatives but they appear to be necessary tradeoffs.

For symbolic memory, the concretization strategy assigns the smallest values to pointer memory and avoids assignment of the same value to different pointers.

BootStomp was able to find 5 vulnerabilities in bootloader code in a short amount of time (albeit with a 12-Core 128GB machine) so the results look promising.

Also the code is on [GitHub](https://github.com/ucsb-seclab/BootStomp), kudos for open source release of tools in academia.
