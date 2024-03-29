---
title: wLaunchELF 4.43x_isr
subtitle: a mod of the most famous FileBrowser for the PlayStation2, with some tweaks and enhacements to provide the most complete and stable version of wLE out there
tags: [PS2, wLaunchELF, PSX-DESR]
header_type: splash
header_img: "https://user-images.githubusercontent.com/57065102/226750383-e6b8d934-aa60-4825-95bd-968a898f6e9e.png"
categories: [Projects, Fork]
---

The mod offers the following features compared to upstream wLaunchELF:

- smaller
- more stable
- supports accessing mx4sio devices
- supports detection of PS3/PS4 DualShocks
- supports accessing the PSX-DESR internal flash (`xfrom`)
- supports manipulating memory card folders timestamp to fix/prepare a folder holding opentuna exploit

<script>
function update() {
  var bits = ["https://github.com/israpps/wLaunchELF_ISR/releases/download/latest/BOOT"];
  var form = document.getElementById("form");
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

> choose the features you want and download

<form id="form" method="get" onchange="update()">
  <div id="features">
    <label><input type="checkbox" name="exfat"  data-toggle="tooltip" data-placement="top" title="Support accesing BDM devices with exfat filesystem"> EXFAT</label> <br>
    
    <label><input type="checkbox" name="no_network" data-toggle="tooltip" data-placement="top" title="Network features disabled to reduce space"> No Network</label> <br>
    
    <label><input type="checkbox" name="ds34"  data-toggle="tooltip" data-placement="top" title="Support for detecting PS3 and PS4 controllers"> DS34</label> <br>
    
    <label><input type="checkbox" name="mx4sio" data-toggle="tooltip" data-placement="top" title="Support for browsing MX4SIO devices"> MX4SIO</label> <br>
    
<!---    <label><input type="checkbox" name="xfrom" data-toggle="tooltip" data-placement="top" title="Support for browsing PSX internal flash memory"> XFROM</label> <br> --->
    
  </div>
  <a id="wledl" href="#" class="btn btn-outline-success" onclick="update();return true;">Download</a>
</form>

[Github Repository](https://github.com/israpps/wLaunchELF_ISR){: .btn .btn-outline-primary }
[PSX Place thread](https://www.psx-place.com/resources/wlaunchelf-4-43x_isr.1112/){: .btn .btn-outline-primary }


[wLaunchELF isr for namco arcades](https://github.com/israpps/wLaunchELF_ISR/releases/tag/sys2x6_latest){: .btn .btn-outline-danger }
