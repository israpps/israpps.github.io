---
title: wLaunchELF 4.43x_isr
subtitle: a mod of the most famous FileBrowser for the PlayStation2, with some tweaks and enhacements to provide the most complete and stable version of wLE out there
tags: [PS2, wLaunchELF, PSX-DESR]
---

The mod offers the following features compared to upstream wLaunchELF:

- smaller
- more stable
- supports accessing mx4sio devices
- supports detection of PS3/PS4 DualShocks
- supports accessing the PSX-DESR internal flash (`xfrom`)
- supports manipulating

<script>
function update() {
  var bits = ["https://github.com/israpps/wLaunchELF_ISR/releases/download/latest/BOOT"];
  var form = document.getElementById("form");
  if (form.exfat.checked) bits.push("-EXFAT");
  if (form.ds34.checked) bits.push("-DS34");
  if (form.no_network.checked) bits.push("-NO_NETWORK");
  if (form.xfrom.checked) bits.push("-XFROM");
  if (form.mx4sio.checked) bits.push("-MX4SIO");
  bits.push(".ELF");
  var url = bits.join("");
  form.url.value = url;
}
update();
</script>

Download
> choose the features you want and download

<form id="form" method="get" onchange="update()">
  <div id="features">
    <label><input type="checkbox" name="exfat"> EXFAT</label>
    <label><input type="checkbox" name="no_network"> No Network</label>
    <label><input type="checkbox" name="ds34"> DS34</label>
    <label><input type="checkbox" name="mx4sio"> MX4SIO</label>
    <label><input type="checkbox" name="xfrom"> XFROM</label>
  </div>
  <input readonly name="url">
</form>
