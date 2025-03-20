---
title: PS2 Namco arcade compatibility guide
description: A writeup on how to make PS2 applications compatible with PS2 COH-H models
image: "/assets/pics/COH.jpg"
tags: [PS2, documentation, Namco Arcades]
style: border
color: primary
---

<img src="/assets/pics/COH.jpg" class="wow animated fadeIn" data-wow-delay=".15s" width="1000" height="500" data-toggle="tooltip" data-placement="bottom" title="Namco System 246 Rack C"/>

> first of all, thanks to krHACKen for explaining several things to me.

## RPC Incompatibilities
----
COH-H models are plagued with this issue because their builtin software is based on a newer SDK (3.1.0) than the one used to make builtin software of retail PS2s

### SIO2 related Drivers
----
Unlike retail PS2s. the COH-H models dont have OSD versions of the stock drivers because there is no OSD (so drivers like `rom0:XMCMAN` dont exist)  
please note the following:  
the arcade modules `rom0:MCSERV` and `rom0:PADMAN` both use the OSD RPC. so their RPC service is equivalent to the `rom0:XMCMAN` and `rom0:XPADMAN` from retail PS2.  

If you use those modules make sure to bind the MCSERV RPC like this:
```c
mcInit(MC_TYPE_XMC);
```
and for the PADMAN module. link your application against `libpadx` not `libpad`

### CDVDMAN RPC
----
Unlike retail PS2. the COH-H models do not load the **CDVDMAN** RPC service on boot (`rom0:CDVDFSV`).
therefore. any function that requires access to **CDVDMAN** will hang if you dont execute that module first.  
as soon as you reboot the IOP you should load that module like this:
```c
SifLoadStartModule("rom0:CDVDFSV", 0, NULL, NULL);
```

### File I/O Services
----
the COH-H models builtin **FILEIO** module is not compatible with our homebrew RPCs.  
therefore, we must replace it. and for that, the IOP must be rebooted using an IOPRP image containing homebrew **FILEIO**.  
for simplicity and to avoid surprises I decided to not only replace the **FILEIO** RPC service. I also replace the **IOMAN** module.   
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
----
newlib will try to open `rom0:ROMVER` and use some of the mechacon cmds (wich require CDVDMAN), does that ring a bell? yes, we talked about these two things on the previous paragraph.

As an all in one solution, if your program only reboots the IOP once, I would do the following, delete the code for IOP reboot from your main function or whatever, and instead, add this to your code:
```c
void _ps2sdk_memory_init() {
    while (!SifIopRebootBuffer(ioprp, size_ioprp)) {}; //replace FILEIO
    while (!SifIopSync()) {};
SifLoadStartModule("rom0:CDVDFSV", 0, NULL, NULL);
}
```
(I always add it after at the end of the source file holding `main()`):

this function is a weak definition found on the PS2SDK crt0, we can use it to gain code execution before newlib does things that will cause issues, giving us the change to replace FILEIO and load CDVDFSV so that newlib can do it's thing without issues 

## extra tips that need more testing/information
- ...

## Game display
----
if your homebrew will need video output, you must use `ACJVLOAD.IRX` module to init the display 

## Memory Card access
----
the COH-H models use two sets of magicgate keys. the port 1 is authenticated with the arcade magicgate keystore. therefore it only reads `COH-H10020` security dongles. while port 2 uses retail keys, wich makes it read normal `SCPH-10020` memory cards.

there is only one exception to this rule and it is the Soul Calibur 2 campaign cards. these are normal memory cards that seem to have some sort of encryption both on the console software and card hardware. this results on these campaign cards corrupting if accessed by any driver beyond the DONGLEMAN driver found on the game

accesing both mc0 and mc1 at the same time is imposible with the builtin ROM modules.  
console has two memory card drivers:
- `rom0:MCMAN`: this is actually the DONGLEMAN driver, reads security dongles from `mc0:/`
- `rom0:MCMANO`: the 'O' stands for **O**riginal. this one reads PS2 cards from `mc1:/`  

the only solution to this is to use the DONGLEMAN driver from Bloody Roar 3 or use a homebrew clone of DONGLEMAN


<div class="alert alert-warning" role="alert">

  if you want security dongle access and your application can modify its contents, please add warnings or checks to avoid accidental tampering of the security dongle boot file (`mc0:/boot.bin`). if that file is gone the security dongle will no longer boot

</div>

## Arcade Watchdog
----
The Arcade SYSCON chip has an additional mechanism on it's software wich we know as "the watchdog".  
This system is basically a 5 min. countdown to shutdown the machine.  
This countdown is reset when the mechacon successfully finishes the memory card authentication routine on a `COH-H10020` security dongle (wich mechacon CMD fires this reset will be confirmed soon).  
However, this check is not done all the time. A special IRX module must be executed for this (`rom0:DAEMON`).  
But before running to make your app run this module keep in mind the following:
- `mc0:/` access will be unstable (at least with on-board DONGLEMAN)

the DONGLEMAN module from Bloody Roar 3 not only auths both arcade and retail memory cards. it seems to have an additional semaphore wrapped around the functions involved on the auth reset (most likely to avoid corruption or somethin. yet this needs confirmation) (for the people interested on reversing: search the string `sema hakama` to find the creation of that semaphore).

Once I get my hands on my own COH-H model I'll update the information about this with my findings. however, in the meantime:
- if your app needs to run for more than 5 mins and you need access to `mc0:/` youre screwed
- if your app does **NOT** care for `mc0:/` access run the following modules in order and forget of this issue: `rom0:SIO2MAN`, `rom0:MCMAN`, `rom0:DAEMON`

> `rom0:DAEMON` setups a thread that calls the internal mcman function `McDetectCard2(0,0)` once every 60secs. wich effectively makes the system go through the dongle auth process, satisfying the arcade watchdog.

## Arcade Mechacon
----
The arcade mechacon is quite unique on certain features. being the most interesting one that it uses different magicgate keys to auth memory cards depending on the port. it uses arcade keys on `mc0:` and retail keys on `mc1:`. unlike retail mechacon wich only uses retail keys or developer mechacon wich uses both retail and developer keys on both ports

Another bit of information: although the arcade **SECRMAN** is based on the `secrman_special` from utility discs, wich has the memory card update routine included. it seems like the mechacon CMDs involved on binding updates to cards are stubbed. making update binding only possible via CECHMZ1 (PS3 memory card to usb adapter) or the modified devkits that sony sold to namco back then (wich we've never seen on the homebrew scene). there is also wLaunchELF MECHAEMU and DONGLEBINDER, two brand new homebrews that will allow you to rebind updates to dongle from retail ps2

# C++ issues
If you chose C++ over C for your PS2 app, and you whant arcade compatibility. I have bad news for ya.

For some reason, C++ applications are crashing on arcade hardware, you remember that `_ps2sdk_memory_init()` that I mentioned above, that was useful to solve the incompatibilities while keeping newlib happy? well, the program crashes before crt0 executes that, so good luck finding the issue...

As of 2/01/2025 this problem has not been solved, I will continue to investigate it however.

# Final notes
----
> this article will be filled as time passes. most of this information was either learnt from other devs or discovered via trial and error. all of this without access to arcade hardware (wich will change soon). if any bit of information in here is wrong or incomplete let me know on discord/psx-place!
