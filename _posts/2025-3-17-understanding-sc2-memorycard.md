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

### MCSERV

MCSERV is the RPC service that allows the EE to call MCMAN (DONGLEMAN in this case) exports directly

it seems to be just MCSERV from SCE SDK `3.0.x`. without additional modifications

### DONGLEMAN

DONGLEMAN is a modified version of MCMAN wich has minor modifications to keep up with the changes done to the arcade mechacon, arcade SECRMAN and others.

There are two versions of dongleman. the one found on bootrom `rom0:MCMAN` wich only reads cards on port 0 (dongles) and the game DONGLEMAN. wich reads both ports without issues.

<details>
    <summary>SC2DONGLEMAN Export Table</summary>

function label           | export number
------------------------ | --------------
`mcman_stub`               | 0
`mcman_stub`               | 1
`mcman_stub`               | 2
`mcman_stub`               | 3
`mcman_stub`               | 4
`McDetectCard`             | 5
`McOpen`                   | 6
`McClose`                  | 7
`McRead`                   | 8
`McWrite`                  | 9
`McSeek`                   | 10
`McFormat`                 | 11
`McGetDir`                 | 12
`McDelete`                 | 13
`McFlush`                  | 14
`McChDir`                  | 15
`McSetFileInfo`            | 16
`McEraseBlock`             | 17
`McReadPage`               | 18
`McWritePage`              | 19
`McDataChecksum`           | 20
`McDetectCard2`            | 21
`McGetFormat`              | 22
`McGetEntSpace`            | 23
`McReplaceBadBlock`        | 24
`McCloseAll`               | 25
`mcman_sio2_mtap_get_sl`   | 26
`mcman_stub`               | 27
`mcman_stub`               | 28
`McReadPS1PDACard`         | 29
`McWritePS1PDACard`        | 30
`mcman_stub`               | 31
`mcman_stub`               | 32
`mcman_stub`               | 33
`mcman_stub`               | 34
`mcman_stub`               | 35
`McUnformat`               | 36
`McRetOnly`                | 37
`McGetFreeClusters`        | 38
`McGetMcType`              | 39
`McSetPS1CardFlag`         | 40
`mcman_stub`               | 41
`McGetModuleInfo`          | 42
`McGetCardSpec`            | 43
`McGetFATentry`            | 44
`McCheckBlock`             | 45
`McSetFATentry`            | 46
`McReadDirEntry`           | 47
`Mc1stCacheEntSetWrFlag`   | 48
`McCreateDirentry`         | 49
`McReadCluster`            | 50
`McFlushCache`             | 51
`McSetDirEntryState`       | 52
`mcman_stub`               | 53

</details>
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
