---
layout: post
title:  "Fixing Kernel headers not found for target kernel error when Installing VirtualBox Guest Additions"
date: 2023-11-24 15:13:16 +0800
categories: python
slug: p20231124151316
---


# Fixing `VirtualBox Guest Additions: Kernel headers not found for target kernel`



OS system for testing:  CentOS7



### 1. why this error:

   The version of `kernel-devel` does not match the version of your running `kernel`. 

   You can check them by running `uname -r` and `rpm -q kernel-devel`.

### 2. update the kernel

   follow the [link](https://www.geeksforgeeks.org/how-to-upgrade-linux-kernel-on-centos-7/) 

   or do the following:

   ```bash
   # yum update -y
   # rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
   # yum install https://www.elrepo.org/elrepo-release-7.el7.elrepo.noarch.rpm
   # yum --disablerepo="*" --enablerepo="elrepo-kernel" list available
   
   # yum --enablerepo=elrepo-kernel install kernel-ml-devel kernel-ml -y
   #or:
   # yum –-enablerepo=elrepo-kernel install kernel-lt-devel kernel-lt -y
   
   # reboot 
   ```

  After update the kernel, you can find the not-the-newest version of kernel. 

  Log in and it let you have the kernel & kernel-devel that can achieve:

  `$(uname -r) == $(rpm -q kernel-devel)`

  * make sure to log into the right version of kernel. Note: not the newest one on my centos7.

### 3. Install the "Development Group Tools"

```bash
   # yum groupinstall "Development tools"
```

### 4. Install the VirtualBox_Guest_Additions

  try to install the VirtualBox_Guest_Additions with:

  ```
  sudo ./VBoxLinuxAdditions.run
  ```


The final success log:

```
    # ./VBoxLinuxAdditions.run
    Verifying archive integrity.
    100%
    MD5 checksums are OK. All good
    Uncompressing VirtualBox 7.0.12 Guest Additions for Linux 100%
    VirtualBox Guest Additions installer
    Removing installed version 7.0.12 of VirtualBox Guest Additions.
    Copying additional installer modules
    Installing additional modules
    VirtualBox Guest Additions: Starting.
    VirtualBox Guest Additions: Setting up modules
    VirtualBox Guest Additions: Building the VirtualBox Guest Additions kernel
    modules. This may take a while.
    VirtualBox Guest Additions: To build modules for other installed kernels, run
    VirtualBox Guest Additions:
    /bin/rcvboxadd quicksetup ‹version>
    VirtualBox Guest Additions: or
    VirtualBox Guest Additions:
    /bin/revboxadd quicksetup all
    VirtualBox Guest Additions: Building the modules for kernel
    3.10.0-1160.102.1.el7.x86 64.
    VirtualBox Guest Additions: reloading kernel modules and services
    VirtualBox Guest Additions: kernel modules and services 7.0.12 r159484 reloaded
    VirtualBox Guest Additions: NOTE: you may still consider to re-login if some
    user session specific services (Shared Clipboard, Drag and Drop, Seamless or
    Guest Screen Resize) were not restarted automatically
```

### references:

https://forums.virtualbox.org/viewtopic.php?t=91563

https://www.geeksforgeeks.org/how-to-upgrade-linux-kernel-on-centos-7/

https://developer.aliyun.com/article/1115583

https://www.debugpoint.com/virtualbox-kernel-headers-not-found-error/



