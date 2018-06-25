---
layout: post
title: "#008: Adding a static IP Address to the wifi on the pi!"
published: true
---

More stuff documented elsewhere - harrah!

* <http://elinux.org/RPi_Setting_up_a_static_IP_in_Debian>

-------------
Prerequisites
-------------

01. Rasbian Installed and Setup as per [#007: My default Raspberry Pi!](http://www.finnangelo.com/007-my_default_raspberry_pi/)

------------------
Choose your address
------------------

I set my Raspberry Pi to be `192.168.0.118`, because my network has ip addresses like `192.168.0.*`; **Yours may be different.**

01. On windows, type `windows-r` to open the run menu
02. Open : `cmd` to open the command prompt
03. Type `ipconfig` to see all your ip address-y stuff

---------------
Edit interfaces
---------------

To set up a static IP address so you don't have to use the 
router (or whatever) to find it again, you need to edit the 
interfaces file (after backing it up first!)

```
sudo cp /etc/network/interfaces /etc/network/interfaces.bak
sudo nano /etc/network/interfaces
```

It took a bit of messing about, but here is the changes to the 
interfaces file<delete>, including wifi</delete>:   
[How to set the `interfaces` file](https://www.raspberrypi.org/forums/viewtopic.php?&t=42670)

```
auto lo

iface lo inet loopback
iface eth0 inet static
address 192.168.0.118
netmask 255.255.255.0
gateway 192.168.0.1

<delete>
auto wlan0
allow-hotplug wlan0
iface wlan0 inet static
address 192.168.0.118
netmask 255.255.255.0
gateway 192.168.0.1
wpa-passphrase <the password for the wifi>
wpa-ssid <the SSID for the wifi></delete>

```

Close nano and

```
sudo reboot
```

---------
Set Hosts
---------

On my windows pc, I set the hosts file with an alias - it's just nicer.  
To do this on your windows7 pc, open Notepad++ as admin, then open
`c:\windows\system32\drivers\etc\hosts`

Add 

    192.168.0.118 raspberrypi

It means you can open putty with raspberrypi as the ip address, harrah!
