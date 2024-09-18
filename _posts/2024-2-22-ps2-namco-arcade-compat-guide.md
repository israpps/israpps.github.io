---
title: PS2 Namco arcade compatibility guide
description: A writeup on how to make PS2 applications compatible with PS2 COH-H models
tags: [PS2, documentation, Namco Arcades]
style: border
color: primary
---

> first of all, thanks to krHACKen for explaining several things to me.

## RPC Incompatibilities

COH-H models are plagued with this issue because their builtin software is based on a newer SDK (3.1.0) than the one used to make builtin software of retail PS2s

### SIO2 related Drivers:
Unlike retail PS2s. the COH-H models dont have OSD versions of the stock drivers because there is no OSD (so drivers like `rom0:XMCMAN` dont exist)
please note the following
the arcade modules `rom0:MCSERV` and `rom0:PADMAN` both use the OSD RPC. so their RPC service is equivalent to the `rom0:XMCMAN` and `rom0:XPADMAN` from retail PS2.  

If you use those modules make sure to bind the MCSERV RPC like this:
```c
mcInit(MC_TYPE_XMC);
```
and for the PADMAN module. link your application against `libpadx` not `libpad`

### CDVDMAN RPC
Unlike retail PS2. the COH-H models do not load the **CDVDMAN** RPC service on boot (`rom0:CDVDFSV`).
therefore. any function that requires access to **CDVDMAN** will hang if you dont execute that module first.  
as soon as you reboot the IOP you should load that module like this:
```c
SifLoadStartModule("rom0:CDVDFSV", 0, NULL, NULL);
```

### File I/O Services
the COH-H models builtin **FILEIO** module is not compatible with our homebrew RPCs.  
therefore, we must replace it. and for that, the IOP must be rebooted using an IOPRP image containing both homebrew **IOMAN** and **FILEIO**.  
for simplicity and to avoid surprices I not only replace the **FILEIO** RPC service. I also replace the **IOMAN** module  
you can build your own, or use a [prebuilt ioprp image](https://github.com/israpps/wLaunchELF_ISR/blob/system-2x6-support/iop/__precompiled/IOPRP_FILEIO.IMG)

> this is how a normal IOP Reboot looks like
```c
    while (!SifIopReset("", 0));
    while (!SifIopSync()) {};
```

> this is how you use an IOPRP image
```c
    while (!SifIopRebootBuffer(ioprp, size_ioprp)) {}; //embed the ioprp image with bin2c
    while (!SifIopSync()) {};
```

## newlib port issues
On latest SDK, wich has a newlib port, you will need to add the following code somwhere:
```c
#include <ps2sdkapi.h>
LIBCGLUE_SUPPORT_NAMCO_SYSTEM_2x6();
```
(I always add it after at the end of the source file holding `main()`):

what this macro does is override one of the standard rtc related functions. this is done because this code executes before `main()` where we still haven't executed the **CDVDMAN** RPC to allow comunication with the mechacon (and therefore with the console RTC)

## extra tips that need more testing/information
- NVRAM access seems to make the system hang sometimes. I havent found the cause. could be something else

## Game display
if your homebrew will be capable of running arcade games, make sure that your application also executes the `ACJVLOAD.IRX` module to init the display (pst: archive.org has one)

## Memory Card access

the COH-H models use two sets of magicgate keys. the port 1 is authenticated with the arcade magicgate keystore. therefore it only reads `COH-H10020` security dongles. while port 2 uses retail keys, wich makes it read normal `SCPH-10020` memory cards.

there is only one exception to this rule and it is the Soul Calibur 2 campaign cards. these are normal memory cards that seem to have some sort of encryption both on the console software and card hardware. this results on these campaign cards corrupting if accessed by any driver beyond the DONGLEMAN driver found on the game

accesing both mc0 and mc1 at the same time is imposible with the builtin ROM modules.  
console has two memory card drivers:
- `rom0:MCMAN`: this is actually the DONGLEMAN driver, reads security dongles from `mc0:/`
- `rom0:MCMANO`: the 'O' stands for **O**riginal. this one reads PS2 cards from `mc1:/`
the only solution to this is to use the DONGLEMAN driver from Bloody Roar 3 or use a homebrew clone of DONGLEMAN

> if you want security dongle access and your application can modify its contents, please add warnings or checks to avoid accidental tampering of the security dongle boot file (`mc0:/boot.bin`). if that file is gone the security dongle will no longer boot