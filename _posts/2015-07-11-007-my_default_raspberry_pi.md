---
layout: post
title: "#007: My default Raspberry Pi!"
published: true
---

This is how I setup my Raspberry Pi; this is all documented (lots of times!) elsewhere on 
the interwebs but I'm still going to document here what I do.

This is all the stuff I do before I run an install script from one of my github projects...
o.k. I haven't made it yet, but I will. I've already created the start of it with 
the [FinnAngelo/BuildServer project](https://github.com/FinnAngelo/BuildServer).

-------------------
Installing Raspbian
-------------------

01. Head over to the [RaspberryPi Downloads](http://www.raspberrypi.org/downloads) and download Raspbian
02. You also need to use the [Win32-Disk-Imager](http://www.softpedia.com/get/CD-DVD-Tools/Data-CD-DVD-Burning/Win32-Disk-Imager.shtml)
    to install the image into your SD card.
03. Click `Write` to write the image to the SD card

-----------------------------
Going headless with **Putty**
-----------------------------

01. Plug the RaspPi onto your network
02. Find out the IP Address of the Pi	
	* I used to use the [Advanced IP Scanner](http://www.radmin.com/products/ipscanner/) but it's 
	  easier to just use my router (under mac filtering) - its mac address web page is good enough 
	  and anyway, later on I set the ip of the Pi to a static address.
* Remote in with ssh using Putty - I use [Putty Portable](http://portableapps.com/apps/internet/putty_portable)
	* Hostname/IP Address: `Your IP address`
	* Port: `22`
* The Raspbian initial login is
	* username: `pi`
	* password: `raspberry`

-------------
Initial Setup
-------------

Here's the basics I always do the first time I run the pi

```bash

sudo apt-get update 
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y
sudo raspi-config 

```

Here's a bunch of settings for the config:

* Expand Filesystem to use all boot disk
* Change your password
* Internationalization
	* Don't touch Configuring locales
		* it should stay as `en-gb.UTF-8`	 
	* Change Timezone - Australia > Melbourne
* Advanced
	* Update - takes about 2min

After closing the config down

```bash

sudo reboot
	
```
