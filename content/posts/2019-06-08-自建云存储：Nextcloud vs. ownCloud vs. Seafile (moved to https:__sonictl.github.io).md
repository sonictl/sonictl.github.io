---
layout: post
title: '自建云存储：Nextcloud vs. ownCloud vs. Seafile (moved to https://sonictl.github.io)'
date: 2019-06-08 23:57:00 +0800
category: from_cnblogs
slug: p20190608235700
---
# Self-hosted Cloud Storage: Nextcloud vs. ownCloud vs. Seafile

>Updated at 2020-07-05 11:00:
If you meet usability issues, consider Alternatives: VerySync(微力同步), Resilio Sync, Syncthing, Tresorit, Pydio, ownCloud, rsync, GoodSync. [Why do you need to ditch nextcloud, owncloud, seafile, blabla...](https://www.cnblogs.com/sonictl/p/13221967.html)

>Updated at 2020-05-19 07:36:
I recently used seafile and nextCloud. tried deploying them. But what I've found: the advantage of nextCloud is that the developers seem to be more responsible, the disadvantage is that it doesn't allow users to register their own accounts and requires administrators to open new ones. Seafile's advantage is mainly open registration, light-weighted and simplicity. seafile's developers, to put it mildly, seem slightly amateurish compared to nextCloud's developers. That is, seafile will encounter various problems in the deployment process. I've done tests on both x64 Linux and arm64. It's only on x64 Linux, using Docker to deploy seafile-mc:latest. It's OK to use file synchronization, but there are bugs that fail to upload. As a team of only 5-6 core developers, the developers of seafile are very appreciated. They put a lot of effort into it. We should support them. On OSX, the seafile's client takes RAM about 87MB, however, nextCloud takes 150MB for running. Seafile is a light weighted choise.
>
>If you are a lightweight NAS user, consider the difficulty of deploying, choose nextCloud. nextCloud also has better support for embedded boards such as arm64 (aarch64).
>
>

By [Ashutosh KS](https://www.hongkiat.com/blog/author/ashutosh_ks/) in [Hosting](https://www.hongkiat.com/blog/category/hosting/). Updated on June 13, 2018.

Are you planning to build your own Dropbox-type cloud storage for your team or business? Though there are **various self-hosted cloud solutions** for creating a private cloud yet all of them will not fit your requirements.

That is why, in this post I am going to confront the top three [self-hosted cloud storage solutions](https://www.hongkiat.com/blog/free-tools-to-build-personal-cloud/)i.e. Nextcloud, ownCloud, and Seafile, to help you pick the best. These three are **free and open-source solutions** to create and host private cloud — a cloud for you and your contacts only.

So, let us discuss these in detail to know where they stand individually.

**Read Also:** [15 Tips to Get More Out of Dropbox](https://www.hongkiat.com/blog/dropbox-tips-and-tricks/)

### Introduction

[ownCloud](https://owncloud.org/) was started to provide a **free replacement for proprietary storage** service providers. [Nextcloud](https://nextcloud.com/), on the other hand, is a **featureful fork of ownCloud** that was started by ownCloud’s core developers including its founding developer. And [Seafile](https://www.seafile.com/en/home/) was born with an objective to **develop and distribute a file syncing software**.

Although these three were started to provide a **proprietary-free cloud storage** solution yet they have lots of differences. The most notable among those is, ownCloud and Seafile offer two editions — a server edition that is free and open-source and a pro/enterprise edition with extra features, but Nextcloud, on the other hand, features a **single edition with optional enterprise support**.

### Download options

Nextcloud provides **numerous methods to install or get** it — an archive file and a web installer for dedicated servers and shared hosts. It also offers appliances and images for easy deployment to your servers.

Lastly, there are also **officially suggested cloud providers and device manufacturers** to get it easily.

ownCloud, being the base of Nextcloud, **offers almost similar installation options** — a tarball and a web installer. It also offers appliances, images, and distribution packages to deploy it readily on servers. Moreover, there are various hosting partners to create and sign up for your private cloud quickly.

Seafile gives **less options than the above two** — a web installer (installation script) as well as pre-built binary packages for Linux distributions. Moreover, it offers docker images and **supports Raspberry Pi** as well, interestingly.

**Nextcloud is the clear winner** here. You can get ready-made devices with preinstalled Nextcloud, which are not available for ownCloud. And both of these offer a lot more than Seafile for getting and setting up a private cloud.

### Usability features

#### Sharing features

Nextcloud offers numerous features to enhance collaboration among a team. It supports **Collabora Online Office** to allow viewing and editing documents online.

Moreover, you can search for and **share a file with a user or group** on the cloud, add comments to discuss about it, or create a public link to share it with others. You can also add **a expiration date or a password** to links for added protection.

![Nextcloud has sharing features](https://img2020.cnblogs.com/blog/780771/202106/780771-20210626204238206-733771519.gif)

ownCloud features all sharing features of Nextcloud. What I found unique as well as interesting is its ‘Guests’ feature, which enables creating limited accounts that **allow guests to have full collaboration** without assigning them as members.

![ownCloud has Guests feature](https://img2020.cnblogs.com/blog/780771/202106/780771-20210626204238206-733771519.gif)

Seafile too offers almost the **same features as Nextcloud**. It also avails a library feature to create libraries of files and folders that you can then sync or share.

![Seafile has Library feature](https://img2020.cnblogs.com/blog/780771/202106/780771-20210626204238206-733771519.gif)

**ownCloud surely stands out** in this section with its unique feature ‘Guests’. Of course, both the other solutions provide all the necessary sharing features.

#### Supported devices

Nextcloud desktop clients support **Windows 7 and above, macOS 10.10 and newer versions, and Linux distributions** as well. Its mobile apps are available for Android, iOS, and Windows platforms with the last one still in testing phase.

ownCloud too supports all devices as Nextcloud except for Windows Mobile.

Seafile also supports as many platforms as supported by ownCloud. What makes it unique is it provides **drive and sync clients separately** for desktop platforms.

Though Nextcloud has the edge here yet Windows Mobile platform is less used, thus all the three solutions have almost the **same set of supported devices**.

#### Apps & integrations

Nextcloud offers ‘Nextcloud Talk’ and ‘Nextcloud Groupware’, which promotes collaboration and productivity among a team and makes it a complete solution. Talk allows the users to **text, call, or have web meetings** with other users. Groupware provides **webmail, calendar, and contacts**management features.

Also, its ‘App Store’ hosts 120+ apps to add more functionalities.

![Nextcloud Talk adds call feature](https://img2020.cnblogs.com/blog/780771/202106/780771-20210626204238206-733771519.gif)

ownCloud’s store features official **apps for calendar and contacts**. Its ‘ownCloud Marketplace’ stores 200+ official and third-party apps to extend its functionality.

I found there are apps to add bookmarks and tasks feature, add external storage services including Dropbox, integrate a backup solution, etc.

However, there is **no app to add text, call, or web meetings** support, unlike Nextcloud.

![ownCloud's Contacts app](https://img2020.cnblogs.com/blog/780771/202106/780771-20210626204238206-733771519.gif)

Seafile, on the other hand, misses all such features. That means, it is just a file sync and share platform with support for online office but it **lacks collaboration features** like calendar, contacts, web mail, and text and call as well.

**Nextcloud is definitely the winner here** — it is a mature solution with various collaboration features, making it a self-hosted rival of Google Drive + Hangouts.

ownCloud lacks many such features but is still better than Seafile, which is just a Dropbox-like file storage solution with basic collaboration options.

### Security features

Nextcloud features robust security measures including encryption while data transfer, server-side storage encryption, and client-side **end-to-end encryption**. It also offers file access control and app access rights for better control.

Also, LDAP, SAML, Active Directory, and Kerberos are supported out of the box.

![Nextcloud's security features](https://img2020.cnblogs.com/blog/780771/202106/780771-20210626204238206-733771519.gif)

ownCloud offers **similar security features with some exceptions** to Nextcloud like it does not feature app access rights and Native SAML support. ownCloud even miss content security policy feature, which is offered by Nextcloud.

Seafile also features **server-side data encryption and end-to-end encryption** at client side along with encryption during data transfer. However, it misses features like LDAP and Active Directory, which are found in others.

**Nextcloud is again the victor** in this section. Though all three are good at usual and essential security features yet Nextcloud has lot more features to protect your data from intruders and give admins fine control over its accessors.

### Enterprise features

Nextcloud, as I told before, offers almost **everything in its free edition**. It does include full-text search, anti-virus integration, data workflow management, file access control, audit logs, and integrated account management.

If you opt for one of their subscriptions, they offer you maintenance and support. Moreover, you can opt for **Collabora Online Office or branding services** separately.

![Nextcloud supports Collabora Online Office](https://img2020.cnblogs.com/blog/780771/202106/780771-20210626204238206-733771519.gif)

ownCloud features many productivity and security features in its enterprise edition. Its workflows feature lets you **automate file management**, ownBrander lets you **custom brand your cloud**, and SharePoint integration offers access to all SharePoint files on ownCloud.

Moreover, it offers network drive support, **file firewall** (to set access rules for files), audit logs, single sign-on, and more.

![ownCloud's PDF Viewer app](https://img2020.cnblogs.com/blog/780771/202106/780771-20210626204238206-733771519.gif)

**Seafile too offers numerous features in its pro edition**. You can lock files to avoid co-editing, set permissions on folders, and have **role-based accounts**. If I talk about usability features, it include **full-text file search**, Office Online Server integration, and an online garbage cleaner.

Moreover, it features advanced single sign-on, **remote wipe, anti-virus** integration, and audit log for high security.

Nextcloud is definitely better than the other two here — especially if you are looking for **a low-cost yet featureful solution**.

It avails most features in its free edition and its subscription cost is lowest with ownCloud’s being highest.

### Which to choose?

It is your decision at the end of the day, but I will suggest to evaluate your team’s or business’s requirements with the features provided by these solutions. Then you can **choose one of these based on the best match**. Don’t you agree?

### Table of comparison

|                     | Nextcloud                                                    | ownCloud                                                     | Seafile                                                      |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Download options    | Web installer, Archive file, Appliances and images, Distribution packages, Cloud providers, Ready-made devices | Web installer, Archive file, Appliances and images, Distribution packages, Cloud providers | Web installer, Archive file, Docker images, Distribution packages |
| Sharing features    | Online Office, Share with a user or group, Public yet protected links | Online Office, Share with a user or group, Public yet protected links, Guests feature | Online Office, Share with a user or group, Public yet protected links, Library feature |
| Supported devices   | Windows, macOS, Linux, Android, iOS, Windows Mobile          | Windows, macOS, Linux, Android, iOS                          | Windows, macOS, Linux, Android, iOS                          |
| Apps & integrations | Talk & Groupware apps, App store with 120+ apps              | App store with 200+ apps                                     | None                                                         |
| Security features   | Storage encryption, End-to-end encryption, File access control, App access rights, LDAP, Native SAML, Active Directory, Kerberos | Storage encryption, End-to-end encryption, File access control, LDAP, Active Directory, Kerberos | Storage encryption, End-to-end encryption, App access rights, LDAP, Shibboleth, Active Directory, Kerberos |
| Enterprise features | Full-text search, Anti-virus integration, Data workflow management, File access control, Audit logs, Integrated account management, Collabora Online Office, Custom branding | Full-text search, Anti-virus integration, Data workflow management, File firewall, Network drive support, Audit logs, Single sign-on, SharePoint integration, Collabora Online Office, Custom branding | Full-text search, Lock files, Anti-virus integration, Remote wipe, Audit logs, Role-based account management, Office Online Server, Custom brandin |


read more: [Stream media files from nextcloud to your Android (and iOS) device with Kodi](https://ownyourbits.com/2018/09/16/stream-media-files-from-nextcloud-to-your-android-and-ios-device-with-kodi/)
read more: [Why do you need to ditch nextcloud, owncloud, seafile, blabla...](https://www.cnblogs.com/sonictl/p/13221967.html)