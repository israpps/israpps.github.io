---
name: PROVERB
description: alternative bootloader to Sony's PSALM "boot.bin" for arcade PS2s
tools: [System246, System256, Konami Python]
style: border
color: primary
---

# PROVERB

Proverb is an alternative bootloader for Arcade PS2s (COH-H model series and their direct derivative units)

the idea behind this is to use proverb as a fixed `boot.bin` file, then have whatever program you want to boot first as a plain ELF wich you can update more comfortably.

also, it could be used to reduce boot times on original security dongle's from games.  
this bootloader is already smaller than the original, and you could also remove `mc0:IOPRP214A` since proverb does not use external IOPRP images


## Usage
the program can be built to look for the elf file to run from a config file. but it can also be hardcoded, specified at compile time, like the psalm bootloader from sony

## installation

We cant give you fully functional files, you have to encrypt and bind these files by yourself, wich you can do by using kelftool to encrypt and then either donglebinder or the twinsign method to bind the update file to a security dongle

### How to encrypt
```cmd
kelftool encrypt dongle INPUT_ELF OUTPUT_KELF --keys=arcade --apptype=7 --mgzone=0x03
```

## FAQ

- Why is it called Proverb?

> Because the official sony bootloader was part of the sony "Psalm" library. and since Proverb is the first bible book after Psalm, I thought it was neat to use that as name.

# usage examples
{% include elements/video.html id="CcfiiEYZP-g" %}

this is a recording made by User Franco23444
where we can see **Ridge Racer: Arcade Battle**

with the original bootloader, the game took approximately 42 seconds to reach the video init stage.  
while the Proverb bootloader allowed the same stage to be reached in 18 seconds

[Download](https://github.com/PS2Homebrew-arcade/proverb/releases/tag/nightly){: .btn .btn-outline-primary }
[Github Repository](https://github.com/PS2Homebrew-arcade/proverb){: .btn .btn-outline-primary }


