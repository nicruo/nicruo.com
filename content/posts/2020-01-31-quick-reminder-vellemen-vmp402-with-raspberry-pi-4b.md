+++ 
date = 2020-01-31T12:25:00+00:00
title = "Quick Reminder: Vellemen VMP402 with Raspberry Pi 4B"
aliases = ['/2020/01/31/quick-reminder-vellemen-vmp402-with-raspberry-pi-4b', '/2020/01/31/quick-reminder-vellemen-vmp402-with-raspberry-pi-4b.html']
+++

On the Raspberry Pi 4B, add to the end of the file `/boot/config.txt`:

```
max_usb_current=1
hdmi_group=2
hdmi_mode=87
hdmi_cvt 800 480 60 6 0 0 0
hdmi_drive=1
```
and comment the line

```
dtoverlay=vc4-fkms-v3d
```

Result:

```
#dtoverlay=vc4-fkms-v3d
```