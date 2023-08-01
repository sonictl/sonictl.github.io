---
layout: post
title: 'How to Share Wired Internet Via Wi-Fi and Vice Versa on Linux'
date: 2020-07-09 09:27:00 +0800
category: from_cnblogs
slug: p20200709092700
---
# How to Share Wired Internet Via Wi-Fi and Vice Versa on Linux

[Aaron Kili](https://www.tecmint.com/author/aaronkili/)April 15, 2020 Categories[Networking Commands](https://www.tecmint.com/category/networking-commands/) [Leave a comment](https://www.tecmint.com/share-internet-in-linux/#respond)

In this article, you will learn how to share a **wired** (**Ethernet**) internet connection via a wireless hotspot and also how to share a wireless internet connection via a wired connection on a Linux desktop.

This article requires you to have at least two computers: a Linux desktop/laptop with a wireless card and an Ethernet port, then another computer (which may not necessarily be running Linux) with either a wireless card and/or an Ethernet port.

### Sharing Wired(Ethernet) Internet Connection Via Wi-Fi Hotspot

First, connect your computer to a source of internet using an **Ethernet** cable as shown in the following screenshot.

[![Connect Ethernet Cable](https://www.tecmint.com/wp-content/uploads/2020/04/connect-ethernet-cable.png)](https://www.tecmint.com/wp-content/uploads/2020/04/connect-ethernet-cable.png)Connect Ethernet Cable

Next, enable **Wireless** connections, then go to **Network Settings** as highlighted in the following screenshot.

[![Enable Wireless Connection](https://www.tecmint.com/wp-content/uploads/2020/04/enable-wireless-connections.png)](https://www.tecmint.com/wp-content/uploads/2020/04/enable-wireless-connections.png)Enable Wireless Connection

Then click **Use as Hotspot** as shown in the following screenshot.

[![Use as Hotspot Feature](https://www.tecmint.com/wp-content/uploads/2020/04/use-as-hotspot-feature.png)](https://www.tecmint.com/wp-content/uploads/2020/04/use-as-hotspot-feature.png)Use as Hotspot Feature



Next, from the pop-up window, click **Turn On** to activate the wireless hotspot.

[![Turn On Hotspot](https://www.tecmint.com/wp-content/uploads/2020/04/trun-on-hotspot.png)](https://www.tecmint.com/wp-content/uploads/2020/04/trun-on-hotspot.png)Turn On Hotspot

Now a wireless hotspot should be created with a name defaulting to the hostname e.g **tecmint**.

[![Wireless Hotspot Created](https://www.tecmint.com/wp-content/uploads/2020/04/wireless-hotspot-created.png)](https://www.tecmint.com/wp-content/uploads/2020/04/wireless-hotspot-created.png)Wireless Hotspot Created

Now you can connect another computer or device via the hot-spot to the internet.

### Sharing Wi-Fi Internet Connection via Wired(Ethernet) Connection

Start by connecting your computer to a wireless connection with access to the internet e.g **HackerNet** in the test environment. Then connect an **Ethernet** cable to it and go to **Network Connections**.

[![Connect to Wireless and Enable Ethernet](https://www.tecmint.com/wp-content/uploads/2020/04/connect-to-wireless-connection-and-enable-ethernet-connection.png)](https://www.tecmint.com/wp-content/uploads/2020/04/connect-to-wireless-connection-and-enable-ethernet-connection.png)Connect to Wireless and Enable Ethernet

From the pop-up window, select the **Wired/Ethernet** connection, then go to its **settings** as described in the following screenshot.

[![Select Network Connection Interface](https://www.tecmint.com/wp-content/uploads/2020/04/network-connections-interface.png)](https://www.tecmint.com/wp-content/uploads/2020/04/network-connections-interface.png)Select Network Connection Interface

Under the connection settings, go to **IPv4** Settings.

[![Select IPv4 Settings](https://www.tecmint.com/wp-content/uploads/2020/04/wired-connection-ipv4-settings.png)](https://www.tecmint.com/wp-content/uploads/2020/04/wired-connection-ipv4-settings.png)Select IPv4 Settings

Under the IPv4 settings, set the **Method** to **Shared** to other computers as shown in the following screenshot. Optionally, you can add the IP address to define the network to use. Then click **Save**.

[![Select Share to Other Computers](https://www.tecmint.com/wp-content/uploads/2020/04/ipv4-connection-method.png)](https://www.tecmint.com/wp-content/uploads/2020/04/ipv4-connection-method.png)Select Share to Other Computers

Next, turn the wired connection **off** then **on**, to activate it once more. Then open it under **Network Connections**, it should now be configured for sharing (by having a default IP address of **10.42.0.1**) as shown in this screenshot.

[![Sharing Wired Connection with Other Computers](https://www.tecmint.com/wp-content/uploads/2020/04/wired-connection-shared-to-other-computers.png)](https://www.tecmint.com/wp-content/uploads/2020/04/wired-connection-shared-to-other-computers.png)Sharing Wired Connection with Other Computers

**Note**: You can also share a bridged interface the same way as the wired interface as shown in the following screenshot.

[![Share a Bridged Interface with Other Computers](https://www.tecmint.com/wp-content/uploads/2020/04/share-a-bridged-interface-to-other-computers.png)](https://www.tecmint.com/wp-content/uploads/2020/04/share-a-bridged-interface-to-other-computers.png)Share a Bridged Interface with Other Computers

Go ahead and connect another computer to the other end of the Ethernet cable or an access point to serve many computers/devices. For any inquiries, reach us via the feedback form below.