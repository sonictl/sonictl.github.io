---
layout: post
title: 'ubuntu上virtualbox无法找到usb设备【解决】'
date: 2017-07-30 12:21:00 +0800
category: from_cnblogs
slug: p20170730122100
---
<h1><a class="question-hyperlink" href="https://askubuntu.com/questions/25596/how-to-set-up-usb-for-virtualbox">How to set up USB for Virtualbox?</a></h1>
<h3>USB in different versions of Virtual Box</h3>
<p>For use of USB in Virtual Box&nbsp;<strong>3.x</strong>&nbsp;you need a&nbsp;<a href="http://www.virtualbox.org/wiki/VirtualBox_PUEL" rel="noreferrer">PUEL-version</a>. From Virtual Box&nbsp;<strong>&gt; 4.x</strong>&nbsp;USB 1.0 is supported in the OSE version installed from software center. For&nbsp;<strong>USB 2.0</strong>&nbsp;or&nbsp;<strong>USB 3.0</strong>&nbsp;(from Virtual Box &gt; 5.x) we need to install an extension pack free for&nbsp;<a href="http://www.virtualbox.org/wiki/Downloads" rel="noreferrer">download from Oracle</a>. This will make our Virtual Box a PUEL-Version (see&nbsp;<a href="https://askubuntu.com/questions/41478/how-do-i-install-the-closed-source-version-of-virtualbox/41487#41487">this question</a>&nbsp;on details on how to install Virtual Box from the Oracle repository).</p>
<p>To change settings of a virtual machine needs the guest to be powered off.</p>
<h3>Become a "vboxuser" <span style="color: #ff0000;">！！</span></h3>
<p>To be able to get access to an attached USB device the Ubuntu&nbsp;<strong>host</strong>&nbsp;user needs to be in the&nbsp;<code>vboxusers</code>&nbsp;group. This can be done from&nbsp;<strong><a href="https://askubuntu.com/questions/66718/how-to-manage-users-and-groups">Users and Groups</a></strong>&nbsp;after having installed the&nbsp;<a href="http://apt.ubuntu.com/p/gnome-system-tools" rel="noreferrer">gnome-system-tools</a>&nbsp;<a href="http://apt.ubuntu.com/p/gnome-system-tools" rel="noreferrer"><img src="https://hostmar.co/software-small" alt="Install gnome-system-tools" /></a>&nbsp;or from the command line by</p>
<pre><code>sudo usermod -aG vboxusers &lt;username&gt; 
</code></pre>
<p>We need a reboot or logout/login for group membership to take effect. On a Windows host a special kernel module will provide USB access.</p>
<h3>Activate USB support in Virtual Box Manager</h3>
<p>We need to activate the virtual USB driver in our guest OS.</p>
<blockquote>
<p><strong>Note</strong>&nbsp;that we can only change these settings when the virtual machine is in shut down state.</p>
</blockquote>
<p>In the USB settings from Virtual Box Manager tick&nbsp;<em>"Enable USB Controller"</em>&nbsp;For enabling the USB 2.0 driver also tick&nbsp;<em>"Enable USB 2.0 (EHCI) Controller"</em>.</p>
<p><img src="https://i.stack.imgur.com/cPIyW.png" alt="enter image description here" /></p>
<h3>Select host USB device for access from the guest</h3>
<p>To grant access to USB devices we need to select a device to&nbsp;<strong>disable in the host</strong>&nbsp;and to&nbsp;<strong>enable in the guest</strong>&nbsp;(this is a precaution to avoid simultaneous access from host and guest). This can be done from the panel&nbsp;<em>Devices</em>&nbsp;menu or by right mouse click in the bottom panel of the Virtual Box Manager on the USB icon:</p>
<p><img src="https://i.stack.imgur.com/gyf5D.png" alt="enter image description here" /></p>
<p>Tick the device you need in the guest, untick it if you need it in the host. The selected device will immediately be accessible from the guest. A Windows guest may need additional drivers:</p>
<p><img src="https://i.stack.imgur.com/OgFLu.png" alt="enter image description here" /></p>
<ul>
<li>Windows 7 needs an&nbsp;<a href="https://downloadcenter.intel.com/product/65855/Intel-USB-3-0-eXtensible-Host-Controller-Driver" rel="noreferrer">additional driver</a>&nbsp;for USB 3.0 support.</li>
<li>Windows 10 does not accept an NTFS formatted USB pen drive.</li>
</ul>
<h3>Use USB filters for permanent access in the guest</h3>
<p>By defining USB filters we can define USB devices that will automatically be presented to the guest when booting the guest OS.</p>
<p><img src="https://i.stack.imgur.com/aPBHN.png" alt="enter image description here" /></p>
<p>Click on the green&nbsp;<strong>+</strong>&nbsp;symbol on the right to add a known device.</p>
<blockquote>
<p>Note, that some devices may lead to a boot failure of the guest. We can not use these devices for filters.</p>
</blockquote>
<p>Read more on USB support in the&nbsp;<a href="https://www.virtualbox.org/manual/ch03.html#idp19116496" rel="noreferrer">Virtual Box User Manual</a>.</p>