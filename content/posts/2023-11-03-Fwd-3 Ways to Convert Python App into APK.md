---
layout: post
title:  "3 Ways to Convert Python App into APK"
date: 2023-11-03 21:44:13 +0800
categories: embedded
slug: p20231103214413
---

# 3 Ways to Convert Python App into APK

When you write an android app with python ([see how to](https://towardsdatascience.com/building-android-apps-with-python-part-1-603820bebde8)), with packages like `kivy`, it is suffering that convert your Python code into installable/runable APK file for real mobile Android devices.

This blog is borrowing [Kaustubh Gupta's blog](https://towardsdatascience.com/3-ways-to-convert-python-app-into-apk-77f4c9cd55af) and is re-edited to introduce the 3 ways to obtain the APK from Python Android app code.

## 1. The challege

we need to package the python code properly, otherwise, Python apps build with Kivy cannot be directly transferred to android devices.

This conversion process is only possible on a Linux system (for Dec 2020) as the main components of this conversion, `buildozer` and `python-for-android` are currently supported on Linux based systems only.

Other challenges include failed app conversions, app crashing on the start, or not able to connect to the internet. Some issues need extra attention, this post will provide 3 different ways to successfully convert the Python app to APK.

## 2. The flow of the conversion work

Before moving ahead, let’s look at the flow of the conversion:

1. Making sure that the app entry point file is named as `main.py`

2. Installing the dependencies.

3. Initialize the `buildozer`

4. Edit the `specs file`

5. Start the process

The 1,2 and 4 don’t require any explicit change but it is necessary to understand how to configure the buildozer `specs` properly.

The `buildozer spec file` is automatically generated while initializing the buildozer.

There are a few lines that need to be modified in that file before proceeding (There are a total of 339 lines in the actual file):

```yaml
# (str) Title of your application 
title = <Nice tile of the app>

# (str) Package name
package.name = <A package name of your app>

# (str) Package domain (needed for android/ios packaging), start with com or org
package.domain = <Name this as: com.<any name> >

# (list) Application requirements,  list the requirements of your app
# comma separated e.g. requirements = sqlite3,kivy
requirements = python3,kivy, kivymd, <More supported requirements can be added here>

# uncomment the internet permissions line in case your app requires it.
# (list) Permissions
android.permissions = INTERNET <commnet, uncomment depending upon use case>
```

**ps**: use `kivy` and `kivymd` defined modules to avoid any app crashing error.

### Introduction for the three ways:

 1. The Google Colab way: install packages and run buildozer in Google Colab.
 2. The github workflow way: write an operation workflow script for github webside.
 3. The Linux way: Virtual Machine/Virtual Private Server

The network env seems not robust for all the in-firewall users. Thus some users in some nations may need an oversea virtual machine that running Linux to finish the APK packaging procedures.

Below are the three ways that you can use:

### 1. The Google Colab Way!

Google Colab provides the required Linux environment. The Colab provides a virtual machine with 75GB HD space, 12GB RAM and 12GB GPU power! Machine-learning and Data-mining will cheers.

You can even use other users' shared colab notebook to do you task.

Here is a step-by-step note for APK packaging:

```bash
! pip install buildozer

! pip install cython==0.29.19

!sudo apt-get install -y \
    python3-pip \
    build-essential \
    git \
    python3 \
    python3-dev \
    ffmpeg \
    libsdl2-dev \
    libsdl2-image-dev \
    libsdl2-mixer-dev \
    libsdl2-ttf-dev \
    libportmidi-dev \
    libswscale-dev \
    libavformat-dev \
    libavcodec-dev \
    zlib1g-dev

!sudo apt-get install -y \
    libgstreamer1.0 \
    gstreamer1.0-plugins-base \
    gstreamer1.0-plugins-good

!sudo apt-get install build-essential libsqlite3-dev sqlite3 bzip2 libbz2-dev zlib1g-dev libssl-dev openssl libgdbm-dev libgdbm-compat-dev liblzma-dev libreadline-dev libncursesw5-dev libffi-dev uuid-dev libffi6

!sudo apt-get install libffi-dev

!buildozer init

!buildozer -v android debug

!buildozer android clean

```

The notebook code is originally provided by [Machine Learning Hub](https://www.youtube.com/channel/UCgyQ4pSntDf9hw9Rv4hmNBA). Check out their [video](https://www.youtube.com/watch?v=mUdnjNGePZw&t=5s)!

Before running the cells, make sure to upload your app code to the colab notebook and after running the bulldozer init command, make sure to edit the specs file generated and nothing else!

This is the easiest and most convenient way to build apps without the need for an actual system! But some details like specs file editing are in the chaper `The Linux way` below.

### 2. The Linux way (triky but detail)

This is the little harder way but this should be the first very obvious way.

The process of setting up the dependencies, transferring the data from the host system to the Linux machine is time-consuming.

In the terminal,

```bash
wget https://github.com/HeaTTheatR/KivyMD-data/raw/master/install-kivy-buildozer-dependencies.sh
chmod +x install-kivy-buildozer-dependencies.sh

./install-kivy-buildozer-dependencies.sh
```
*More details about the works done in the .sh file you just launched*: [this link](https://towardsdatascience.com/python-for-android-start-building-kivy-cross-platform-applications-6cf867d44612)

After the script is done installing, navigate to your app directory and run:

```bash
buildzoer init
```
This will create the buildozer spec file which consists of all configurations you can modify. After you are done editing this file, run:

```bash
buildozer android debug
```

For the first time, this command will take longer than usual to execute (14–19 minutes) and after that, you will have a bin folder in the app directory with the APK. Transfer the app to any android device and see if works!



You can also run these installations in WSL (Windows Subsystem Linux) in Windows 10 if you don’t want to opt for a virtual box setup.

### 3. The github workflow way

github also provides a Linux system but only when the process is getting executed. Don’t know much about this? After you are done with this article make sure to check out [this article](https://towardsdatascience.com/github-action-that-automates-portfolio-generation-bc15835862dc)!

GitHub actions run on a Linux system and therefore you can configure the various parameters for the conversion.

Github Actions are the piece of software that lets you build, test, and deploy your code right from your GitHub repository. You can schedule jobs for various events occurring in your GitHub repository such as pushing new code, commits, or opening an issue. You can schedule either on happens of these events or use a corn job to set a timer.

The Kivy developers have created these actions and you just need to include this workflow in your .github/workflows/<any name>.yml file:

```yaml
name: Build
on:
  push:

jobs:
# Build job. Builds app for Android with Buildozer
build-android:
  name: Build for Android
  runs-on: ubuntu-latest

  steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: Build with Buildozer
      uses: ArtemSBulgakov/buildozer-action@v1
      id: buildozer
      with:
        workdir: <specify the directory of the app no don't mention this the app files are in root directory>
        buildozer_version: stable

    - name: Upload artifacts
      uses: actions/upload-artifact@v2
      with:
        name: package
        path: ${{ steps.buildozer.outputs.filename }}
```

In this method, you need to place the buildozer spec file also with your app files. You can find any spec file online and after that just copy-paste that, edit it, upload the files to the repository and then add this workflow. 

After you run this workflow, your app will be created as an artifact that can be easily downloaded. You can also configure it to upload the APKs on another branch of the repository. Check out the official [GitHub page](https://github.com/ArtemSBulgakov/buildozer-action) for all the configurable parameters.

## Exaples and conclusions:

Reference: [3 ways to convert python app into apk](https://towardsdatascience.com/3-ways-to-convert-python-app-into-apk-77f4c9cd55af#Any%20Examples?)

