---
title: Cracking the SoulCalibur II Conquest Memory card
description: A writeup on how i've been struggling with this little piece of history. the second rarest memcard in the world (IMO)
image: https://israpps.github.io/assets/pics/conquest_card_pan.jpg
tags: [blog, Namco Arcades, Conquest Card]
style: border
color: danger
---

<div id="banner" style="overflow: hidden; align-items: center; float: left;">
    <div class="" style="max-width: 40%; max-height: 40%; display: inline-block;">
        <img src ="/assets/pics/conquest_card_front.jpg" class="wow animated fadeIn">
    </div>
    <div class="" style="max-width: 40%; max-height: 40%; display: inline-block;">
        <img src ="/assets/pics/conquest_card_back.jpg" class="wow animated fadeIn">
    </div>
</div>

## History

This memory card was used by the namco system 246 release of SoulCalibur II for storing the progress of the "Conquest Mode".

A game mode exclusive to this release of the game.

The memory cards used come from the recall campaign sony did during PS2 launch on japan. these cards, had a bug on their controller chips (usually `CXD9585R` & `CXD9585AR`) that corrupted the FAT table of the card filesystem.

SoulCalibur II recycles these cards using a custom filesystem (or raw data?) encrypted page-by-page. as well as custom ECC format (it seems)

## Magic String

This card uses a different magic string

```Memory Card for SoulCaliburII (C)1995 1998 2002 NAMCO LTD.```

## Related files

Based on talks with ExtraWeb1. it seems like the following files inside the SoulCalibur II Dongle are directly related to the conquest card

```
mc0:PSAC04A
mc0:PSAC05
```

## Related drivers

Soulcalibur2 Dongles carry the MCSERV module on the dongle root. and the dongleman module goes inside one of the 3 IOPRP images found on the dongle, more specifically: `mc0:IOPRPACM`

Both Dongleman and MCSERV have all the same MD5 Hash across the 3 versions of SC2
```
SC21, Ver.A10
SC22, Ver.A10
SC23, Ver.A10
```

## Timeline

Events related to new discoveries about the card will be placed here, to keep a chronological order

<div class="col mt-4">
  <div class="timeline-body bg-themed">
    {% for item in site.data.conquesttimeline %}
      <div class="timeline-item">
        <div class="content">
          <h2>{{ item.title }}</h2>
          <h6 class="date">{{ item.date }}</h6>
          <p>{{ item.description }}</p>
        </div>
      </div>
    {% endfor %}
  </div>
</div>
