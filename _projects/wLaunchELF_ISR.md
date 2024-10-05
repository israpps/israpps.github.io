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
- special build for namco system 246/256

<script>
function update() {
  var bits = ["https://github.com/israpps/wLaunchELF_ISR/releases/download/latest/BOOT"];
  var form = document.getElementById("features");
  if (form.exfat.checked || form.mx4sio.checked) bits.push("-EXFAT");
  if (form.ds34.checked) bits.push("-DS34");
  if (form.no_network.checked) bits.push("-NO_NETWORK");
  if (form.mx4sio.checked) bits.push("-MX4SIO");
  bits.push(".ELF");
  var url = bits.join("");
  document.getElementById("wledl").setAttribute("href",url);
}
update();
</script>

## Download

> tick the desired features before clicking the download button

<div id="features" class="btn-group" role="group" aria-label="Program features" onchange="update()">
  <input type="checkbox" class="btn-check" id="exfat" autocomplete="off">
  <label class="btn btn-outline-primary" for="exfat">EXFAT</label>

  <input type="checkbox" class="btn-check" id="no_network" autocomplete="off">
  <label class="btn btn-outline-primary" for="no_network">No network</label>

  <input type="checkbox" class="btn-check" id="ds34" autocomplete="off">
  <label class="btn btn-outline-primary" for="ds34">DS34</label>

  <input type="checkbox" class="btn-check" id="mx4sio" autocomplete="off">
  <label class="btn btn-outline-primary" for="mx4sio">MX4SIO</label>
</div>

<!---    <label><input type="checkbox" name="xfrom" data-toggle="tooltip" data-placement="top" title="Support for browsing PSX internal flash memory"> XFROM</label> <br> --->

<a id="wledl" href="#" class="btn btn-outline-success" onclick="update();return true;">Download</a>



[Github Repository](https://github.com/israpps/wLaunchELF_ISR){: .btn .btn-outline-primary }


[PSX Place thread](https://www.psx-place.com/resources/wlaunchelf-4-43x_isr.1112/){: .btn .btn-outline-primary }

<p class="d-inline-flex gap-1">
  <button class="btn btn-outline-warning" type="button" data-bs-toggle="collapse" data-bs-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
    Special Builds
  </button>
</p>

<div class="collapse" id="collapseExample">
  <div class="card card-body">


[wLaunchELF isr for namco arcades](https://github.com/israpps/wLaunchELF_ISR/releases/tag/sys2x6_latest){: .btn .btn-outline-danger }

  </div>
</div>

