---
permalink: /useqinfo/useq-update/
title: "How to Update uSEQ Firmware"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true
---

## Firmware?

Firmware is the software that runs on the small computer in the uSEQ module. It interprets LISP code, and manages the hardware. It lives in a flash memory inside the module.

## Updates

uSEQ is a continually evolving system.  When there's an update to the firmware, the editor will show a message saying the update is available and provide a link.

Firmware updates are shown on the [github releases page](https://github.com/Emute-Lab-Instruments/uSEQ/releases)



## How do I install the firmware?

1. Download the .uf2 file from github. 
2. You need to put the module into 'boot select' mode.  There are two holes in the top right hand section of the panel, labelled ```bset``` and ```reset```.  You can push a small stick or pin into these holes to push the buttons underneath.  Connect to the module with USB, and then hold down ```bsel``` while pressing and releasing ```reset```.
3. The module will now appear in your operating system file manager
4. Drag the .uf2 file onto the module.  This will update the firmware and reboot the module.


