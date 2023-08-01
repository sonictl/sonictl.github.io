---
layout: post
title: 'Deluge: Enables BT download on your Raspberry Pi'
date: 2020-07-06 08:46:00 +0800
category: from_cnblogs
slug: p20200706084600
---
> 写在前面：Arm板卡上的 BT 下载器得可以通过web进行操作才比较方便。所以人家推荐了 Deluge 这款工具。 
> references: [Installing Deluge on the Raspberry Pi](https://pimylifeup.com/raspberry-pi-deluge/)
>                     [7 Best BitTorrent Clients for Linux in 2020](https://www.fossmint.com/best-bittorrent-clients-for-linux/)

# Installing Deluge on the Raspberry Pi

 by Emmet Jul 30, 2019 Updated May 03, 2020 [Beginner](https://pimylifeup.com/category/projects/beginner/)

In this Raspberry Pi Deluge project, we will show you how to set up the popular Deluge torrent client on the Pi.

![Raspberry Pi Deluge Thumbnail](https://pi.lbbcdn.com/wp-content/uploads/2019/07/Raspberry-Pi-Deluge-Thumbnail.jpg)

Throughout this tutorial, we will be showing you how to install and configure the Deluge torrent client alongside the Deluge web interface. We will also show you how to enable Deluge’s remote access ability.

As well as showing you how to install and setup the software on your Raspberry Pi we will also be showing you how you can setup Deluge as a service.

Setting Deluge up on your Raspberry Pi as a service will allow it so that the software will automatically start when the Raspberry Pi boots up.

There are VPNs that you can setup on the Raspberry Pi to help with privacy. I recommend using something [like ExpressVPN](https://pimylifeup.com/raspberry-pi-expressvpn/) or [VyprVPN](https://pimylifeup.com/raspberry-pi-vyprvpn/). There are plenty others that you can install.

For the maximum amount of storage you might want to [mount an external hard drive](https://pimylifeup.com/raspberry-pi-mount-usb-drive/). You also might want to [setup a Raspberry Pi NAS](https://pimylifeup.com/raspberry-pi-nas/) so you can access your files remotely.

##  Equipment List

Below is all the equipment that I made use of for this Raspberry Pi Deluge tutorial.

### Recommended

 [Raspberry Pi](https://go.pimylifeup.com/l8KF94/amazon/raspberrypi) 2, 3 or 4

 [Micro SD Card](https://go.pimylifeup.com/DUVENo/amazon/microsdcard)

 [Power Supply](https://go.pimylifeup.com/TwjJnF/amazon/powersupply)

 [Ethernet Cord](https://go.pimylifeup.com/9YIU76/amazon/ethernetcord) or [WiFi dongle](https://go.pimylifeup.com/89vmLk/amazon/wifidongle) (The Pi 3 and 4 has WiFi inbuilt)

### Optional

 [Raspberry Pi Case](https://go.pimylifeup.com/vbWKKX/allraspberrypicases)

##  Setting up Deluge on the Raspberry Pi

Firstly, we will be using the [latest Raspbian operating system](https://pimylifeup.com/how-to-install-raspbian/) anything else you might run into problems.

**1.** Before we install the Deluge torrent client on to the Raspberry Pi, we should first ensure all our packages are up to date. You can do this by running the following two commands.

```
sudo apt update
sudo apt upgrade
```

**2.** In this step, we will now install the [Deluge torrent client](https://deluge-torrent.org/) to the Raspberry Pi.

The version of the Deluge torrent client that we will be installing contains its web interface. The Deluge web interface will allow you to manage your torrents from any web browser easily.

To install Deluge, deluges web interface module and the python module the web interface requires we run the command below.

```
sudo apt install deluged deluge-web deluge-console python-mako
```

**3.** If you want to store your downloaded torrents in a different location, then you can now go ahead and create the required folder.

In our example, we will be creating a folder within a drive we mounted to our “**/media/NASHDD**” folder called “**torrent-downloads**“. This folder is a [Samba share](https://pimylifeup.com/raspberry-pi-samba/), and our **pi** user already has the privileges to write to this directory.

We can create this folder by running the following command.

```
mkdir /media/NASHDD/torrent-downloads
```

**4.** Let’s now start-up and then shutdown the **Deluged daemon** by running the following two commands. This process will make the Deluge torrent software create the config files we need on our Raspberry Pi.

We will modify two config files so that we can allow remote access to Deluge and create a user to login to the Deluge daemon.

```
deluged
sudo pkill -i deluged
```

**5.** We can easily insert a user into Deluges auth file by running the command below.

Make sure you swap out certain parts of this command such as “``” with the username you want to set and “``” with the password that you want to give the user.

```
echo "<USERNAME>:<PASSWORD>:10" >> ~/.config/deluge/auth
```

**6.** With the authentication now modified we need to go ahead and fire up the Deluge software again.

Without the software running we won’t be able to make the next configuration change.

```
deluged
```

**7.** Now we can go ahead and change Deluge’s config file so that the “`allow_remote`” variable is set to “`True`“.

We can do this by making use of the “`deluge-console`” package and running the command below.

```
deluge-console "config -s allow_remote True"
```

**8.** With all the required configuration changes done, we can now go ahead and fire up the Deluge web interface.

Run the following command to launch the web interface. We use “`-f`” on “`deluge-web`” to launch it as a daemon.

```
deluge-web -f
```

**9.** Lastly, let’s grab the Raspberry Pi’s local IP address with the command below. We will need this IP so that we can access the Raspberry Pi Deluge web interface.

```
hostname -I
```

##  The Deluge Web Interface

**1.** With the Deluge web interface up and running on the Raspberry Pi, we can now access it by going to the Pi’s IP address followed by the “**8112**” port.

Enter the following web address, make sure you swap out “****” with your Pi’s IP address.

```
http://<IPADDRESS>:8112
```

**2.** Upon going to the web address, you will be greeted by Deluge’s web interface.

To access the interface, you will be first asked to enter a password (**1.**). The web interface password by default is “**deluge**“. We will update this password later on in the tutorial.

Once you have entered the password for Deluge, press the “**Login**” button (**2.**).
![Raspberry Pi Deluge Web Login Screen](https://pi.lbbcdn.com/wp-content/uploads/2019/07/01-Deluge-web-interface-login-screen.png)

**3.** After logging into the Deluge web interface, you will then be shown the “**Connection Manager**” (**1.**). Click the available host and then click the “**Connect**” button (**2.**).

![Deluge Connection Manager](https://pi.lbbcdn.com/wp-content/uploads/2019/07/02-Deluge-web-interface-connection-manager.png)

**4.** Once you have selected a connection in the “**Connection Manager**” box, you will now be brought to the main Deluge web interface screen.

If you want to change the download location or the default password, you can now do this by opening up the preferences dialog box by clicking “**Preferences**” in the toolbar.

![Deluge Preferences Location](https://pi.lbbcdn.com/wp-content/uploads/2019/07/05-Deluge-web-interface.png)

**5.** The first screen within the Deluge preferences interface is the “**Downloads**” screen. This screen lets you set things such as the download folder (**1.**)

Within this interface, you can specify the general download folder. You can also pick more specific options such as a location you want the completed torrents kept.

If you want to change the default password for the Deluge web interface, then you will need to go to the “**Interface**” screen (**2.**).

![Deluge Web Download Settings](https://pi.lbbcdn.com/wp-content/uploads/2019/07/03-Deluge-web-Preferences.png)

**6.** Now within the “**Interface**” screen, you can set the password (**1.**).

First, you will need to specify the original password in the “**Old password**” textbox, which in a first-run case, this will be “**deluge**“.

Next, specify your desired password in both the “**New Password:**” and “**Confirm Password:**” text fields.

Once you are all set, click the “**Change**” button (**2.**)

![Deluge Web Change Password](https://pi.lbbcdn.com/wp-content/uploads/2019/07/04-Deluge-web-Change-Password.png)

##  Setting up Deluge as a service

**1.** Now we need to make a service file for the Deluge daemon. This service file is what [systemd will read](https://wiki.debian.org/systemd) from so that it knows how to manage the daemon.

Run the following command to begin writing our “**deluged.service**” file.

```
sudo nano /etc/systemd/system/deluged.service
```

**2.** Within this file, enter the following text.

These settings will set it up so that Deluged daemon will be started after the network has come online.

We also set it so that the default “**pi**” user starts the service. Setting the user helps prevent any potential permission issues.

```
[Unit]
Description=Deluge Daemon
After=network-online.target

[Service]
Type=simple
User=pi
Group=pi
UMask=007
ExecStart=/usr/bin/deluged -d
Restart=on-failure
TimeoutStopSec=300

[Install]
WantedBy=multi-user.target
```

**3.** Once done, you can go ahead and save the Deluged service file by pressing **CTRL+ X** then **Y** followed by **ENTER**.

**4.** Now that we have created our Deluged service file let’s go ahead and enable it. Enabling the service means that on the next boot, it will start automatically.

Enable the service by running the following command.

```
sudo systemctl enable deluged.service
```

We won’t be running the service straight away because the “**deluged**” daemon will be running already from the previous sections of this tutorial.

**5.** Next, we will need to create another service that will manage running the deluge web interface for us.

We will be calling this service file “**deluge-web.service**“, we can begin writing this file by running the following command on the Raspberry Pi

```
sudo nano /etc/systemd/system/deluge-web.service
```

**6.** Within this file, enter the following text.

This text defines our Deluge web interface service. This service will only start up after both the network has come online and the deluged service is active.

Like our other service, we also set this so that the “**pi**” user will run the deluged web server.

```
[Unit]
Description=Deluge Web Interface
After=network-online.target deluged.service
Wants=deluged.service

[Service]
Type=simple
User=pi
Group=pi
UMask=027
ExecStart=/usr/bin/deluge-web
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**7.** With our “**deluge-web.service**” now created, let’s go ahead and enable it by running the command below.

```
sudo systemctl enable deluge-web.service
```

**8.** Finally, we need to restart our Raspberry Pi Deluge setup by running the following command.

Restarting our Raspberry Pi will allow us to test that our services are running as they should be.

```
sudo reboot
```

**9.** Once your Raspberry Pi has finished rebooting you can now check to ensure that both Deluged and the Deluge web service is up and running.

Run the following two commands to retrieve the current status of the two services.

```
sudo systemctl status deluged
sudo systemctl status deluge-web
```

If the status returns as “**Active: active (running)**” then you have successfully set up the Deluge torrent client on your Raspberry Pi.

There are other [alternative torrent software packages](https://pimylifeup.com/raspberry-pi-torrentbox/) for the Raspberry Pi that you can utilize. Each has its pros and cons, so it’s entirely up to you which one you pick.

By the time you’re at this point in this Raspberry Pi Deluge tutorial, you should have everything configured and set up correctly. If you’re running into issues, then please don’t hesitate to leave a comment below.