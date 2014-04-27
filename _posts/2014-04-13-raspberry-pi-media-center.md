---
layout: post
category: pi
tags: [raspberry-pi]
---

## Raspberry Pi as a Media Center

In 2013, I bought a Raspberry Pi with the goal of using it as a media center. I also bought other accessories, the full list is the following:

- [Raspberry Pi](http://www.amazon.com/RASPBERRY-MODEL-756-8308-Raspberry-Pi/dp/B009SQQF9C/) *US$ 44.00*
- [Case](http://www.amazon.com/SB-Raspberry-Pi-Case-Clear/dp/B008TCUXLW/) *US$ 13.50*
- [16 GB SD Class 10](http://www.amazon.com/Transcend-Class-Flash-Memory-TS8GSDHC10E/dp/B003VNKNEG/) *US$ 13.50*
- [HDMI Cable](http://www.amazon.com/AmazonBasics-High-Speed-HDMI-Cable-Meters/dp/B003L1ZYYM) *US$ 6.00*
- [USB HUB 7 port](http://www.amazon.com/Plugable-Port-Speed-Power-Adapter/dp/B003Z4G3I6/) *US$ 21.00*
- [WIFI+Bluetooth dongle](http://www.amazon.com/Cirago-Bluetooth-Speed-Adapter-BTA7300/dp/B005QUQPDA/) *US$ 30.00*
- [IR Remote Control](http://www.amazon.com/SANOXY%C2%AE-Wireless-Remote-Control-Mouse/dp/B0050PUGZE/) *US$ 15.00*
- External hard drive, monitor and keyboard (already had)


The USB HUB I have is also used as power source for the RP. You can see in the image below how things are connected:

![My Raspberry Pi]({{ site.url }}/assets/raspberry-pi1.jpg) Raspberry Pi Setup: USB Hub, Hidden Raspberry Pi and HD

For software I am using the distro [Raspbmc](http://www.raspbmc.com/download/). It is based on Raspibian, a debian distro for Raspberry Pi. So it comes with *apt* and you can download packages from debian repositories. *Raspbmc* includes [XBMC](http://xbmc.org/), which is a cool interface for using it as a media center. Also there are some nice services such as samba, FTP, SSH and SFTP.

### Wifi

I have struggle a bit to configure the wifi. So, I am providing below the links I've used as a resource:

- [http://anup.info/blog/2012/07/29/raspbmc-wifi](http://anup.info/blog/2012/07/29/raspbmc-wifi)
- [http://wiki.debian.org/WiFi/HowToUse](http://wiki.debian.org/WiFi/HowToUse)
- [http://pt.kioskea.net/faq/2545-configurar-o-wifi-no-linux](http://pt.kioskea.net/faq/2545-configurar-o-wifi-no-linux)
- [http://www.raspberrypi.org/phpBB3/viewtopic.php?f=27&t=7592](http://www.raspberrypi.org/phpBB3/viewtopic.php?f=27&t=7592)

Also here it is my */etc/network/interfaces* configuration file. Note that I am using static IP address.

    iface lo inet loopback #localhost config

    auto eth0 #ethernet config
    iface eth0 inet static
        address 192.168.2.3
        netmask 255.255.255.0
        gateway 192.168.2.1 #use this IP in the PC when connecting

    allow-hotplug wlan0
    iface wlan0 inet static #wi-fi config
        wpa-ssid wirelessname #place your wifi ssid
        wpa-psk wirelesspassword #place your password
        address 192.168.1.100
        netmask 255.255.255.0
        gateway 192.168.1.1

    iface default inet dhcp
    
### Backup

It is possible to backup the entire SD card. Plug the SD card in your Linux or OSX and run the following command:

	dd if=/dev/sdx of=/path/to/image bs=1M

Where /dev/sdx is the SD card device point.

### Other tips

Some other details of my configuration:

- I have a 16 GB SD card, but 8 GB would also be fine.
- Media files are stored in the external hard drive.
- XBMC has a [subtitles plugin](http://wiki.xbmc.org/?title=Subtitles), so you can easily download subtiles for the video you are watching.
- Use the app [Yatse](https://play.google.com/store/apps/details?id=org.leetzone.android.yatsewidgetfree) in android to control XBMC. It is not just a remote control, you can navigate through your library in it.
- Use [ramlog](http://maild.org/wordpress/?p=57) to make SD card last longer.
- Stream from XBMC, trough Android, to a Chromecast, by using Yatse or [BubbleUPnP](https://play.google.com/store/apps/details?id=com.bubblesoft.android.bubbleupnp).
- The hard drive I use stops spinning if it is not used for a while.

### Other ideas

Here are some ideas that I have, but did not tried yet:

- Buy a full [keyboard remote control](http://www.amazon.com/iPazzport-Wireless-Entertainment-Handheld-Keyboard/dp/B0093GTVNO).
- Backup only XBMC configuration.
- Use as server for backup using [backintime](http://backintime.le-web.org/), [timemachine](http://www.apple.com/support/timemachine/) or [Déjà Dup](https://launchpad.net/deja-dup).
- Host a protected web site with an albums of photos.
- Download (backup) e-mails using [getmail](http://pyropus.ca/software/getmail/).
- Connect a webcam and use as securtiy camera.
