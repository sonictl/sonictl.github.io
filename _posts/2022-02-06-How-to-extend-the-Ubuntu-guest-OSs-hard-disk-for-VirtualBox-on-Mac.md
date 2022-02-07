---
layout: post
title:  "How to extend the Ubuntu guest OS's hard disk for VirtualBox on Mac"
date: 2022-02-06 23:11:02 +0800
categories: virtualbox
slug: p20220206231102
---

# How to extend the Ubuntu guest OS's hard disk for VirtualBox on Mac

## Steps:

- resize the VDI file on Mac OS: [How to Resize a VirtualBox VDI or VHD File on Mac OS X](https://osxdaily.com/2015/04/07/how-to-resize-a-virtualbox-vdi-or-vhd-file-on-mac-os-x/)

- boot the Ubuntu with the image(ISO) and Gparted(resize the partition). 

  **Note:** firstly resize the extension partition (/dev/sda2), then resize the partition (/dev/sda5)

  [Ubuntu VM in VirtualBox: How to increase the size of a disk and make small(er) exports for distribution](https://technology.amis.nl/platform/virtualization-and-oracle-vm/ubuntu-vm-virtualbox-increase-size-disk-make-smaller-exports-distribution/)

- reboot the ubuntu with local vdi file and check the result.

## references:

[How to Resize a VirtualBox VDI or VHD File on Mac OS X](https://osxdaily.com/2015/04/07/how-to-resize-a-virtualbox-vdi-or-vhd-file-on-mac-os-x/)

[Ubuntu VM in VirtualBox: How to increase the size of a disk and make small(er) exports for distribution](https://technology.amis.nl/platform/virtualization-and-oracle-vm/ubuntu-vm-virtualbox-increase-size-disk-make-smaller-exports-distribution/)

[How to Partition, Format, and Mount a Disk on Ubuntu 20.04 Using Command Line](https://www.tecklyfe.com/how-to-partition-format-and-mount-a-disk-on-ubuntu-20-04/)





