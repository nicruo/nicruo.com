+++ 
date = 2019-06-19T16:15:00+01:00
title = "Quick Reminder: Set Android Emulator DNS Server"
aliases = ['/2019/06/19/quick-reminder-set-android-emulator-dns-server', '/2019/06/19/quick-reminder-set-android-emulator-dns-server.html']
+++

Go to your sdk emulator folder and run: 

`emulator -avd [name_of_emulator] -dns-server [dns_server]`

example:

`emulator -avd Nexus_5_API_28 -dns-server 8.8.8.8`

If you are on a Windows machine, you can use "emulator.exe".