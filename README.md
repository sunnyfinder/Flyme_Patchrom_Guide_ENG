# Flyme 6 Patchrom Guide (English)

Auther:FlymeDev_Finder(XDA:sunnyfinder)

System environment:Ubuntu

# Establishing a Build Environment

Installing the JDK</b>

Run the following:

    $ sudo apt-get update

    $ sudo apt-get install openjdk-8-jdk

--------
<b>Installing required packages</b>

     $ sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip

--------

<b>Configuring USB Access</b>

Under GNU/Linux systems (and specifically under Ubuntu systems), regular users can't directly access USB devices by default. The system needs to be configured to allow such access.

The recommended approach is to create a file at /etc/udev/rules.d/51-android.rules (as the root user).

To do this, run the following command to download the 51-android.txt file attached to this site, modify it to include your username, and place it in the correct location:

    $ wget -S -O - http://source.android.com/source/51-android.txt | sed "s/<username>/$USER/" | sudo tee >/dev/null /etc/udev/rules.d/51-android.rules; sudo udevadm control --reload-rules

--------

<b>Installing Repo</b>

Repo is a tool that makes it easier to work with Git in the context of Android. For more information about Repo, see the Developing section.

To install Repo:

Make sure you have a bin/ directory in your home directory and that it is included in your path:

    $ mkdir ~/bin

    $ PATH=~/bin:$PATH

--------
Download the Repo tool and ensure that it is executable:

    $ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

    $ chmod a+x ~/bin/repo

--------

<b>Installing Android Studio</b>

In order to build patchrom project, you must have Android Studio installed.

Download Link：https://developer.android.com/studio/index.html

Install Instructions：https://developer.android.com/studio/install.html

--------

<b>Add the SDK tools and platform-tools to PATH</b>

    $ sudo gedit .bashrc

Add the line:

    export PATH=$PATH:/home/$username$/Android/Sdk/tools:/home/username/Anroid/Sdk/platform-tools

Save and exit. Then run the following:

    $ source .bashrc

--------

<b>Downloading the Source</b>

Use repo init with "-b" for download the branch you want

Use 

    $ repo sync
    
to upgrade to the latest code from flyme git

To initialize your local repository using the patchrom trees, use a command like this:

    $ mkdir Flyme

    $ cd Flyme

    $ repo init -u https://github.com/FlymeOS/manifest.git -b marshmallow-6.0

Then to sync up:

    $ repo sync -c -j4

If the connection has failed or the download code is too slow, use the following command:

    $ repo init --repo-url git://github.com/FlymeOS/repo.git -u https://github.com/FlymeOS/manifest.git -b marshmallow-6.0 --no-repo-verify

    $ repo sync --no-clone-bundle -c -j4

# Port to your Device

After downloading the code, run the following to initialize the build environment:

    $ source build/envsetup.sh

Create new device’s folder (Such as: demo), the later work will be done in this folder:

    $ mkdir -p devices/demo

    $ cd devices/demo

Make sure your device has root privilege or a rooted kernel (preferred), and connect your phone to your computer, then open the phone’s USB debug mode. Then run the following to build a full Flyme patchrom :

<b>Generate device’s config file – Makefile</b>

The command will attempt to automatically extract boot.img and recovery.img from the device. If the extraction fails, you need to copy boot.img and recovery.img into the device’s folder, and then re-execute the command.Then you should modify the Makefile to config the correct device’s information

    $ flyme config      

<b>Generate new device’s project folder</b>

This will generate a stockrom.zip, flash this zip in recovery mode to ensure it works normally

    $ flyme newproject

Automatically add the Flyme code into kernel and framework

    $ flyme patchall    

<b>Resolve conflicts</b>

Automatic patching may cause code merge conflicts. The conflict will be marked in the following form. You must resolve these conflicts in the device's folder.

    <<<<<<< VENDOR

     Vendor Code
  
    =======

      Flyme Code
  
    >>>>>>> BOSP

<b>Generate the completed Flyme ROM package</b>

    $ flyme fullota

<b>Flyme Version Upgrade</b>

Run the following to upgrade Flyme version for your device</b>

    $ flyme cleanall

    $ make clean

    $ flyme upgrade

# International Developer Feedback Channel

1. Github: Put new issue at Github (https://github.com/FlymeOS/manifest/issues/new)

2. Send an email to the Flyme Patchrom Engineer-Mr. Zou Junhua (zoujunhua86@gmail.com) with your log

3. Flyme Developer Hub(Telegram Group): https://t.me/joinchat/F29EBUL0GBexLoHo1gMTsg

# Chinese Developer Feedback Channel

446108651@qq.com

zoujunhua86@gmail.com


# The official certified Flyme download link of third party devices (In Chinese)

http://www.flyme.cn/firmware.html
