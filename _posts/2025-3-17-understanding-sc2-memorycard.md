---
title: Cracking the SoulCalibur II Conquest Memory card
description: A writeup on how i've been struggling with this little piece of history. the second rarest memcard in the world (IMO)
image: https://israpps.github.io/assets/pics/conquest_card_pan.jpg
tags: [blog, Namco Arcades, Conquest Card]
style: border
color: danger
---

<img class="wow animated fadeIn" data-wow-delay=".15s" src="/assets/pics/conquest_card_front.jpg" width="500" height="500"/><img class="wow animated fadeIn" data-wow-delay=".15s" src="/assets/pics/conquest_card_back.jpg" width="500" height="500"/>

## History

This memory card was used by the namco system 246 release of SoulCalibur II for storing the progress of the "Conquest Mode".

A game mode exclusive to this release of the game.

The memory cards used come from the recall campaign sony did during PS2 launch on japan. these cards, had a bug on their controller chips (usually `CXD9585R` & `CXD9585AR`) that corrupted the FAT table of the card filesystem.

SoulCalibur II recycles these cards using a custom filesystem (or raw data?) encrypted page-by-page. as well as custom ECC format (it seems)

## The Software

The DONGLEMAN module that SoulCalibur II uses to handle this card does not seem to be a special version. since more than 70 dongles have a DONGLEMAN module with the same hash (`0a6ca23ae87aba1d0a12641435f84327`) (for example: Bloody Roar 3)

## Magic String
This card uses a different magic string

```Memory Card for SoulCaliburII (C)1995 1998 2002 NAMCO LTD.```

## Related files

Based on talks with ExtraWeb1. it seems like the following files inside the SoulCalibur II Dongle are directly related to the conquest card
```
mc0:PSAC04A
mc0:PSAC05
```

## Next steps

I will soon get one. when I have it. will try to modify the SoulCalibur II dongle contents to get logs and information from the card, everything logged into the ACUART (builtin RS232 on the System246)