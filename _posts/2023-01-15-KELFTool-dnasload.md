---
title: KELFTool (Dnasload Fork)
subtitle: Command line tool to convert PS2 ELF executables into KELF executables
tags: [PS2, Windows, MAC, Linux, CLI]
categories: [Projects, Fork]
---

This KELFTool fork adds a new encryption mode called `DNASLoad`.
This mode creates a special KELF that functions the same way that the already existing mode `fmcb`, wich created KELF files to be binded as memory card updates.

but with a very important difference:...

unlike `fmcb`, `DNASLoad` creates a KELF that can be encrypted and decrypted by both PS2 and PSX-DESR. making this DNASLoad mode the perfect one for binding updates for [KELFBinder](./2022-12-21-KELFBinder.md)



<div class="card border-danger mb-3" style="max-width: 20rem;">
   <div class="card-header">Header</div>
   <div class="card-body">
      <h5 class="card-title">Credits</h5>
      <p class="card-text">Thanks to krHACKen, who shared the DNASLoad User Header to make this possible</p>
   </div>
</div>


[GitHub Repository](https://github.com/israpps/KELFTool){: .btn .btn-outline-primary }
[Download](https://www.psx-place.com/resources/kelftool-dnasload-fork.1319){: .btn .btn-outline-primary }
[PSX Place thread](https://www.psx-place.com/resources/kelftool-dnasload-fork.1319/download?version=2470){: .btn .btn-outline-primary }