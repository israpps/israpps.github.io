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

The memory cards used come from the recall campaign sony did during PS2 launch on japan. these cards, had a bug on their controller chips (usually `CXD9585R` & `CXD9585AR`) that corrupted the FAT table of the card filesystem.

SoulCalibur II recycles these cards using a custom filesystem (or raw data?) encrypted page-by-page. as well as custom ECC format (it seems)

## Magic String

This card uses a different magic string

```Memory Card for SoulCaliburII (C)1995 1998 2002 NAMCO LTD.```

the first block of the conquest card only holds this magic string. all the rest is filled (byte filler is `0xFF` usually)


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

<table>
    <tr>
        <th>function label</th>
        <th>export number</th>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>0</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>1</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>2</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>3</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>4</td>
    </tr>
    <tr>
        <td>McDetectCard</td>
        <td>5</td>
    </tr>
    <tr>
        <td>McOpen</td>
        <td>6</td>
    </tr>
    <tr>
        <td>McClose</td>
        <td>7</td>
    </tr>
    <tr>
        <td>McRead</td>
        <td>8</td>
    </tr>
    <tr>
        <td>McWrite</td>
        <td>9</td>
    </tr>
    <tr>
        <td>McSeek</td>
        <td>10</td>
    </tr>
    <tr>
        <td>McFormat</td>
        <td>11</td>
    </tr>
    <tr>
        <td>McGetDir</td>
        <td>12</td>
    </tr>
    <tr>
        <td>McDelete</td>
        <td>13</td>
    </tr>
    <tr>
        <td>McFlush</td>
        <td>14</td>
    </tr>
    <tr>
        <td>McChDir</td>
        <td>15</td>
    </tr>
    <tr>
        <td>McSetFileInfo</td>
        <td>16</td>
    </tr>
    <tr>
        <td>McEraseBlock</td>
        <td>17</td>
    </tr>
    <tr>
        <td>McReadPage</td>
        <td>18</td>
    </tr>
    <tr>
        <td>McWritePage</td>
        <td>19</td>
    </tr>
    <tr>
        <td>McDataChecksum</td>
        <td>20</td>
    </tr>
    <tr>
        <td>McDetectCard2</td>
        <td>21</td>
    </tr>
    <tr>
        <td>McGetFormat</td>
        <td>22</td>
    </tr>
    <tr>
        <td>McGetEntSpace</td>
        <td>23</td>
    </tr>
    <tr>
        <td>McReplaceBadBlock</td>
        <td>24</td>
    </tr>
    <tr>
        <td>McCloseAll</td>
        <td>25</td>
    </tr>
    <tr>
        <td>mcman_sio2_mtap_get_sl</td>
        <td>26</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>27</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>28</td>
    </tr>
    <tr>
        <td>McReadPS1PDACard</td>
        <td>29</td>
    </tr>
    <tr>
        <td>McWritePS1PDACard</td>
        <td>30</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>31</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>32</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>33</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>34</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>35</td>
    </tr>
    <tr>
        <td>McUnformat</td>
        <td>36</td>
    </tr>
    <tr>
        <td>McRetOnly</td>
        <td>37</td>
    </tr>
    <tr>
        <td>McGetFreeClusters</td>
        <td>38</td>
    </tr>
    <tr>
        <td>McGetMcType</td>
        <td>39</td>
    </tr>
    <tr>
        <td>McSetPS1CardFlag</td>
        <td>40</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>41</td>
    </tr>
    <tr>
        <td>McGetModuleInfo</td>
        <td>42</td>
    </tr>
    <tr>
        <td>McGetCardSpec</td>
        <td>43</td>
    </tr>
    <tr>
        <td>McGetFATentry</td>
        <td>44</td>
    </tr>
    <tr>
        <td>McCheckBlock</td>
        <td>45</td>
    </tr>
    <tr>
        <td>McSetFATentry</td>
        <td>46</td>
    </tr>
    <tr>
        <td>McReadDirEntry</td>
        <td>47</td>
    </tr>
    <tr>
        <td>Mc1stCacheEntSetWrFlag</td>
        <td>48</td>
    </tr>
    <tr>
        <td>McCreateDirentry</td>
        <td>49</td>
    </tr>
    <tr>
        <td>McReadCluster</td>
        <td>50</td>
    </tr>
    <tr>
        <td>McFlushCache</td>
        <td>51</td>
    </tr>
    <tr>
        <td>McSetDirEntryState</td>
        <td>52</td>
    </tr>
    <tr>
        <td>mcman_stub</td>
        <td>53</td>
    </tr>
</table>

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
