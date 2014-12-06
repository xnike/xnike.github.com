Title: Momentics IDE: Installing on a headless Linux system.
Date: 2014-12-06 23:59
Tags: BB10, BBNDK, BlackBerry 10, Linux, Momentics IDE

Assume you wish to use [Jenkins](http://jenkins-ci.org/) to build your BlackBerry 10 applications on a headless Linux system.

In this case on attempt to install SDK via ./sdkinstall you could face with the following error:
```
Qde: Cannot open display: 
Qde: Cannot open display: 
Qde:
An error has occurred. See the log file
```

This issue could be fixed by in 4 simple steps:

a) install xorg-x11-xauth package

b) uncomment line "#X11UseLocalhost yes" in /etc/ssh/sshd_config

c) reboot system to apply new package and settings

d) reconnect to system via ssh using -X option (enabled X11 forwarding).