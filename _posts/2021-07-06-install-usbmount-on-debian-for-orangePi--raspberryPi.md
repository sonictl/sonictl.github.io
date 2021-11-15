---
layout: post
title:  "install usbmount on debian for orangePi / raspberryPi"
date: 2021-07-06 08:02:01 +0800
categories: orangePi embedded_system
slug: p20210706080201
---

## install usbmount on debian for orangePi / raspberryPi

USBmount was removed from Debian a while back because the version from the repositories no longer works. It's still available in Ubuntu, but it does not work properly in Ubuntu 18.04 and newer. The bug was fixed in the USBmount git though, so you can build your own updated USBmount package that works in both Debian and Ubuntu (and on Raspbian / Ubuntu MATE for Raspberry Pi).

Start by installing Git and downloading the latest [USBmount Git code](https://github.com/rbrito/usbmount):

```
sudo apt install git
git clone https://github.com/rbrito/usbmount
```


Next, install the packages needed to create an USBmount DEB package:

```
sudo apt install debhelper build-essential
```


Now all you have to do is navigate to the folder where you cloned the USBmount Git repository, and build the DEB package:

```
cd usbmount
dpkg-buildpackage -us -uc -b
```





---
**Reference:**

# Automatically Mount USB Drives On Ubuntu Or Debian Server With USBmount

[ Logix](https://www.blogger.com/profile/03026963810377267607) Updated on [April 4, 2019](https://www.linuxuprising.com/2019/04/automatically-mount-usb-drives-on.html) [console](https://www.linuxuprising.com/search/label/console?max-results=14), [debian](https://www.linuxuprising.com/search/label/debian?max-results=14), [how-to](https://www.linuxuprising.com/search/label/how-to?max-results=14), [ubuntu](https://www.linuxuprising.com/search/label/ubuntu?max-results=14), [usb](https://www.linuxuprising.com/search/label/usb?max-results=14)



**If you want to automatically mount USB drives on a server running Debian or Ubuntu (including Raspbian or Ubuntu MATE for Raspberry Pi) you can use a simple, but very effective tool called [USBmount](https://github.com/rbrito/usbmount)**.

USBmount is a set of scripts used to automatically mount USB mass storage devices when they are plugged in. While it's not created to only run on servers, USBmount is especially useful on a server because it doesn't have a graphical user interface, and it doesn't depend on any desktop environment. And most desktops can already automount USB devices.

By default USBmount automatically mounts USB devices using `/media/usb0`, `/media/usb1`, ..., `/media/usb7` mount points, with `/media/usb0` being the first plugged in USB device, `/media/usb1` being the second USB device you plugged in, and so on.

For USB sticks that come with a model name, a symbolic link is created at `/var/run/usbmount/MODELNAME` for its mount point.

The default USBmount configuration is set to automount USB devices with vfat, ext2, ext3, ext4 and hfsplus filesystems. NTFS can be enabled after installing USBmount - by editing the `/etc/usbmount/usbmount.conf` configuration file and adding `ntfs fuseblk` to the `FILESYSTEMS` variable (without removing the other filesystem types).



## Alternatives for usbmount:

[udevil](https://github.com/IgnorantGuru/udevil)