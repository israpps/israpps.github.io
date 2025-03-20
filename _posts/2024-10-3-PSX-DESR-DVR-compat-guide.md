---
title: PlayStation X compatibility notes
description: A writeup on how to make PS2 applications support the PSX DESR
image: "https://upload.wikimedia.org/wikipedia/commons/f/fa/Console_psx.jpg"
tags: [PS2, documentation, PSX]
style: border
color: primary
---


<img class="wow animated fadeIn" data-wow-delay=".15s" src="https://upload.wikimedia.org/wikipedia/commons/f/fa/Console_psx.jpg" width="1000" height="500"/>

The PSX DESR is a fusion between a PS2 based upon the `SCPH-50xxx` series and a DVR

It has certain details that you should cover if making a program compatible with it:

### Built-in OSD Font
----
All retail PS2s have a basic font used by the OSDSYS to render the menu text. it's located at `rom0:FONTM`.

However, since the PSX has no menu on the OSDSYS, this file was removed. resulting in errors if your program depends on that file...

### Disc reader
----
The PSX has a dual laser system consisting of:
- the DVR laser
- the PS2 laser

the system is designed to assume whole tray to be broken if the DVR laser is broken/failing/unresponsive/etc

to circunvent this, most apps can force the tray into "PS2 Mode". however, it's not that simple. it requires replacing the CDVDMAN module with the XMB version (`rom0:PCDVDMAN`).

doing so requires using an IOPRP image ([prebuilt ioprp](https://github.com/israpps/PlayStation2-Basic-BootLoader/raw/refs/heads/main/embed/ioprp.img) - [ioprp contents](https://github.com/israpps/PlayStation2-Basic-BootLoader/tree/main/embed#ioprp-internals)). as shown in the below example
```c
void setTrayToPS2Mode(void) {
    int stat, result;
    SifInitRpc(0); // Initialize SIFCMD & SIFRPC
    while (!SifIopRebootBuffer(psx_ioprp, size_psx_ioprp)) {}; // reboot IOP with IOPRP to get PCDVDMAN
    while (!SifIopSync()) {}; // wait for IOP to be ready
    SifInitRpc(0);
    sceCdInit(SCECdINoD);
    while (sceCdChgSys(2) != 2) {}; // Switch the drive into PS2 mode. wich requires PCDVDMAN to work.
    do {result = sceCdNoticeGameStart(1, &stat);} 
        while ((result == 0) || (stat & 0x80)); // notice game start so the PSX front button acts as a reset button.
    while (!SifIopReset("", 0)) {}; // Reset the IOP normally to get the standard PS2 default modules.
    while (!SifIopSync()) {};
}
```

Both FreeMcBoot and PS2BBL already do this for you. however, this information may prove useful if you're not gonna use those...

### On board Flash memory
----
The PSX has a memory card flash memory connected to the SPEED chip (DEV9 interface).  
This flash memory, known as XFROM, holds the main PSX bootloader, making it a vital component of the system, even more than the HDD (because the bootloader from XFROM is the one launching the HDD MBR.KELF)

This flash memory is not accesible on current homebrew design, homebrew XFROMMAN is having issues accesing, while the PSXs built-in XFROMMAN (`rom0:PFLASH`, `rom0:PXFROMMAN`) freeze when executed due to some incompatibility with homebrew iomanX module

most of the time your apps will not need to (and should avoid to) touch the XFROM contents.

for accesing this chip you can use wLaunchELF 4.43_XFW made by balika011.

### Identifying if console is a PSX
----
You can make your application detect if the console is a PSX just by checking the existence of `rom0:PSXVER`

### Identifying if PSX is 1st or 2nd gen
----
The PSX had two generations:

|   1st gen   |   2nd gen   |
| ----------- | ----------- |
| `DESR-5000` | `DESR-5500` |
| `DESR-5100` | `DESR-5700` |
| `DESR-7000` | `DESR-7500` |
| `DESR-7100` | `DESR-7700` |


this can also be quickly identified from software by checking the ROMVER version field:
- if `ROMVER` starts with `0180` it is a 1st gen model
- if `ROMVER` starts with `0210` it is a 2nd gen model

### The HardDrive
----
The PSX HDD cannot be changed because the DVR Processor (DVRP) is actively checking if it is a legit sony ps2 hdd (either PSX or PS2 HDD). it is checked by issuing commands to the HDD.  
if the hdd is not original, power is cut down and DVRP stops all access to it.  

PS2 (`SCPH-20401`) COULD be used to replace the original HDD only retaining the XMB on 1st gen models (because 2nd gen models don't/can't boot the XMB if the DVR area is not present on the HDD. wich can't be recreated on those because it starts after 40gb of data, wich is the actual total size of original PS2 HDDs)

### Mechacon Crash
----
It is unknown if PSX is capable of producing the mechacon crash like the `SCPH-50xxx` series
