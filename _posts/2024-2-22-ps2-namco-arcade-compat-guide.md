---
title: PS2 Namco arcade compatibility guide
subtitle: A writeup on how to make an application compatible with COH models
tags: [PS2, writing, documentation]
categories: [writing]
---
first of all, thanks to krHACKen for explaining several things to me.

### RPC Incompatibilities

COH models are plagued with this issue because their builtin software is based on a newer SDK than the one used to make builtin software of retail PS2s

here are the needed tweaks I found
## Usage of ROM drivers for the SIO2 peripherals
  + use `rom0:MCMAN`
  + use `rom0:MCSERV`
  + use `rom0:PADMAN` 
  + use `rom0:SIO2MAN` 

## usage of ROM driver for cdfs access
use builtin cdvdfsv module (`rom0:CDVDFSV`) instead of homebrew one.

## usage of the appropiate EE RPC libraries
- use libpadx instead of libpad
- initialize the XMC MCSERV RPC instead of the MC one


## replacement of the main I/O drivers
builtin IOMAN and FILEIO modules are not compatible with our homebrew RPCs.  
therefore, we must replace them. and for that, the IOP must be rebooted using an IOPRP image containing both homebrew IOMAN and FILEIO.  
you can build your own, or use a [prebuilt one](https://github.com/israpps/wLaunchELF_ISR/blob/system-2x6-support/iop/__precompiled/IOPRP_FILEIO.IMG)

### newlib port issues
On latest SDK, wich has newlib port, you will need to override a function from the libcglue like this:
```c
_libcglue_rtc_update() {}
```

Else, your program will hang before even hitting `main()`

### extra tips that need more testing/information
- avoid NVRAM reading as much as possible, seems like depending on where you read it, it hangs.

### Game display
if your homebrew will be capable of running arcade games, make sure that your application also executes the `ACJVLOAD.IRX` module to init the display

### aditional notes
accesing both mc0 and mc1 at the same time is imposible with the ROM modules .console has two memory card drivers:
- `rom0:MCMAN`: this is actually the DONGLEMAN driver, reads security dongles from `mc0:/`
- `rom0:MCMANO`: the 'O' stands for **O**riginal. this one reads PS2 cards from `mc1:/`

if you want security dongle access and your application can modify its contents, please add warnings or checks to avoid accidental tampering of the security dongle boot file (`mc0:/boot.bin`)
