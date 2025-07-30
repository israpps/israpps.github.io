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
Thanks to extraweb1 for the pictures
<br>


{% capture list_items %}
History
Magic String
Related files
Related drivers
Timeline
{% endcapture %}
{% include elements/list.html title="Table of Contents" type="toc" %}


## History

This memory card was used by the namco system 246 release of SoulCalibur II for storing the progress of the "Conquest Mode".

A game mode exclusive to this release of the game.

The memory cards used come from [the recall campaign sony did during PS2 launch on japan](https://www.ign.com/articles/2000/03/08/ps2-launch-ps2-memory-card-recall). these cards, had a bug on their controller chips (usually `CXD9585R` & `CXD9585AR`) that corrupted the FAT table of the card filesystem.

SoulCalibur II recycles these cards using a custom filesystem (or raw data?) encrypted page-by-page. as well as custom ECC format

## Magic String

This card uses a different magic string

```Memory Card for SoulCaliburII (C)1995 1998 2002 NAMCO LTD.```

the first block of the conquest card only holds this magic string. all the rest is filled (byte filler is `0xFF` usually)
the actual data differences begin at card page 32. before this. the contents are exactly the same accross all conquest cards I've seen

## Related files
the following files are directly related and a target to REing

- `mc0:SCSLOAD`: game launcher. setups some stuff and launches PS2AC05 file (looking for it in the security dongle, developer flash and the game DVD. In that order)
- `mc0:PSAC05`: main game binary blob, encrypted and RLE compressed.
- `mc0:IOPRPACM`: decoy IOPRP image containing an unused DONGLEMAN driver

## Related drivers
The SIO2MAN, MCSERV and DONGLEMAN drivers used to interface with the conquest card are embedded inside the PS2AC05 binary blob.

`mc0:IOPRPACM` is actually a decoy. the image is not even used, and the dongleman driver inside is most likely left to confuse

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
