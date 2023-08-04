+++ 
date = 2020-03-26T14:40:00+00:00
title = "Quick Reminder: Android Debug over WIFI"
aliases = ['/2020/03/26/quick-reminder-android-debug-over-wifi', '/2020/03/26/quick-reminder-android-debug-over-wifi.html']
+++

From command line with android sdk tools:

```
adb devices 
```
check if device is connected with USB. Then:

```
adb tcpip 5555
```

Disconnect the USB connection. And use your android device ip:

```
adb connect [android-device-ip]:5555
```

And after check if connect:

```
adb devices
```

Simple right?