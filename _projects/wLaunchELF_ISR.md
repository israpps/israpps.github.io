---
name: wLaunchELF 4.43x_isr
tools: [ps2, fork]
image: https://user-images.githubusercontent.com/57065102/226750383-e6b8d934-aa60-4825-95bd-968a898f6e9e.png
description: a mod of the most famous FileBrowser for the PlayStation 2, with some tweaks and enhacements to provide the most complete and stable version of wLE out there
style: border
color: info
---


# wLaunchELF v4.43x_isr

> a mod of the most famous FileBrowser for the PlayStation 2, with some tweaks and enhacements to provide the most complete and stable version of wLE out there

This was one of my first projects. it began 13/1/2021 and continues untill today

The mod offers the following features compared to upstream wLaunchELF:

- smaller
- more stable
- supports accessing mx4sio devices
- supports detection of PS3/PS4 DualShocks
- supports manipulating memory card folders timestamp to fix/prepare a folder holding opentuna exploit
- informs the real internal HDD status
- special builds for arcade PS2 (namco system 246/256, Konami python1)

<script>
function exfatdep() {
  var form = document.getElementById("form");
  if (form.mx4sio.checked) form.exfat.checked = true
}
function switchmmc(mode) {
  var form = document.getElementById("form");
  if (mode == 0) {
    form.mmce.checked = false;
  } else {
    form.mx4sio.checked = false;
  }
}
function update() {
  var bits = ["https://github.com/israpps/wLaunchELF_ISR/releases/download/latest/BOOT"];
  var form = document.getElementById("form");
  if (form.exfat.checked || form.mx4sio.checked) bits.push("-EXFAT");
  if (form.ds34.checked) bits.push("-DS34");
  if (form.no_network.checked) bits.push("-NO_NETWORK");
  if (form.mx4sio.checked) bits.push("-MX4SIO");
  if (form.mmce.checked) bits.push("-MMCE");
  bits.push(".ELF");
  var url = bits.join("");
  document.getElementById("wledl").setAttribute("href",url);
}
update();
</script>


<form id="form" method="get" onchange="update()">
  <div id="features" class="card border-primary">
  <h5 class="card-header text-primary border-primary">Download</h5>
  <div class="card-body border-primary">
    <p class="card-text">Choose the desired features before downloading</p> <br>
<div class="container">
  <div class="row">
    <div class="col-sm">
      <label><input type="checkbox" role="switch" name="exfat" data-toggle="tooltip" data-placement="top" title="Support accesing BDM devices with exfat filesystem"> EXFAT</label>
    </div>
    <div class="col-sm">
      <label><input type="checkbox" role="switch" name="no_network" data-toggle="tooltip" data-placement="top" title="Network features disabled to reduce space"> No Network</label>
    </div>
    <div class="col-sm">
      <label><input type="checkbox" role="switch" name="ds34"  data-toggle="tooltip" data-placement="top" title="Support for detecting PS3 and PS4 controllers"> DS34</label>
    </div>
  </div>
</div>
<br>

<div class="container">
  <div class="row">
    <div class="col-sm">
      <label><input type="checkbox" role="switch" name="mx4sio" data-toggle="tooltip" data-placement="left" title="Support for browsing MX4SIO devices" onclick="switchmmc(0); exfatdep();"> MX4SIO</label>
    </div>
    <div class="col-sm">
      <label><input type="checkbox" role="switch" name="mmce" data-toggle="tooltip" data-placement="left" title="Support for browsing the SDCard of SD2PSX, MemcardPro2 and similar devices" onclick="switchmmc(1)"> MMCE</label> 
    </div>
    <div class="col-sm">
      SOON
      <!--<label><input type="checkbox" role="switch" name="dvrp" data-toggle="tooltip" data-placement="left" title="Support for browsing the Encrypted DVR Area of a PSX DESR HardDrive"> DVR</label> -->
    </div>
  </div>
</div>
    <br><br><br>
    <a id="wledl" href="#" class="btn btn-outline-primary btn-block progress-bar-striped progress-bar-animated" onclick="update();return true;">Download</a>
  </div>
  </div>

  

</form>

<br>
<div class="btn-group">
  <a href="https://github.com/israpps/wLaunchELF_ISR" class="btn btn-outline-primary" aria-current="page">Github Repository</a>
  <a href="https://www.psx-place.com/resources/wlaunchelf-4-43x_isr.1112/" class="btn btn-outline-primary">PSX Place thread</a>
</div>

<br>

[wLaunchELF isr for Arcade PS2](https://github.com/israpps/wLaunchELF_ISR/releases/tag/arcade){: .btn .btn-outline-danger }
