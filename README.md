Welcome to the SHIELD Open Source + Binary Driver Release.

In this README, you will find sync, build, and flashing instructions.

==========
HowTo Sync:
==========

Syncing this release requires git and the repo tool from Google:
http://source.android.com/source/downloading.html#installing-repo

mkdir ~/shield-open-source
cd ~/shield-open-source
repo init -u git://nv-tegra.nvidia.com/manifest/android/binary.git -b rel-29-r3-partner -m tlk/t210.xml
repo sync -j5


==========
HowTo Build:
==========

Building this release requires a Linux build environment configured to
build Android: http://source.android.com/source/initializing.html

Additionally, you will be required to agree to license terms when extracting
the binary drivers.

cd ~/shield-open-source
export TOP=`pwd`
cd vendor/nvidia/licensed-binaries
./extract-nv-bins.sh
cd $TOP
. build/envsetup.sh
setpaths
lunch <product>-userdebug   (See below table for selecting <product>)
mp dev

------------------------------------------------------------
Sr.No.|           SHEILD               |      <product>    |
------------------------------------------------------------
  1.  | SHIELD Android TV Base         |      foster_e     |
------------------------------------------------------------
  2.  | SHIELD Android TV Premium      |    foster_e_hdd   |
------------------------------------------------------------


==========
HowTo Flash:
==========

Before flashing images from this build to your SHIELD, connect your SHIELD
via USB to the PC where you built this tree.

Next, put your SHIELD into fastboot mode using following method:
SW method:
   Boot to android home screen
   Connect the device to linux system
   Open terminal(on linux)
   Type "adb reboot bootloader" in terminal

HW method:
   Disconnect power cable
   Insert USB OTG cable and make sure to connect other end to a host PC
   Connect power cable to SHIELD
   Quickly start pressing power button for ~3 seconds
   Do not hold the button and connect power supply afterwards
   HDMI TV should be always connected to SHIELD

Alternative method:
   Perform software shutdown on SHIELD by holding Power button for 10 seconds
   Connect USB OTG cable to SHIELD
   Start pressing power button for 3 seconds
   HDMI TV should be always connected to SHIELD

Fastboot menu navigation:
   Single tap on touch button for navigations between different menu.
   Hold down touch button for 4 second and release for selecting any menu

If this is the first time you have done this procedure, you must unlock
your bootloader. To unlock your bootloader, run the following command
in a terminal:

fastboot oem unlock

It will take you to '!!! READ THE FOLLOWING !!!' page.
Two selectable options are available. 'Confirm' and 'Back to menu'.
Select 'Confirm' to unlock the bootloader.

Your device's bootloader should now be unlocked.

To flash images from this build to your SHIELD, run the following commands
from the same terminal where you did your build:

fastboot oem dtbname                  (This will print the DTB name for a given product)

cd $OUT
fastboot flash recovery recovery.img
fastboot flash boot boot.img
fastboot flash system system.img
fastboot flash vendor vendor.img
fastboot flash dtb <DTB file name>    (Use result from "fastboot oem dtbname" in <DTB file name>)
fastboot reboot

