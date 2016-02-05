Building Ubuntu/Debian installation for OrangePI H3 boards using debootstrap
============================================================================

- Read and edit **params.sh** to adjust the parameters to your needs.<br/>
- Run `sudo ./create_image` to build the Linux installation according to settings in params.sh<br/>


ORIGINAL LOBORIS POST

You are welcome to try
    Debian,  Ubuntu, Fedora 22, Kali Linux, Arch Linux, OpenSuse, Gentoo, and Slackware
images for OrangePI H3 boards,

You can also build your own image using build scripts (see bottom of the post).

Download from  Mega  or Google Drive 
Last update: 11/09/2015 14:10 UTC
Last kernel update (scriptbin_kernel.tar.gz): 10/17/2015 13:30 UTC
Last install (desktop & emmc) scripts update (desktop_scripts.tar.gz): 09/02/2015 17:45 UTC

Review of the OPI boards with Debian Jessie XFCE image on CNXSoft (updated with video).
And more about OPI camera and GPIO.


Available images:

<distro_rel>_mini.img                                   basic Debian and Ubuntu images, base for server or desktop, many usefull console programs installed (mc, htop, tmux, ...)
OrangePI_Ubuntu_Vivid_Mate.img              Ubuntu 15.04 with Mate Desktop
OrangePI_Lubuntu_Vivid.img                      Lubuntu 15.04 with LXDE/Lubuntu Desktop
OrangePI_Jessie_Xfce.img                          Debian 8 with XFCE Desktop
OrangePI-PC_Ubuntu_Vivid_Mate.img    prepared for OrangePI users, just copy to SDCard, no configuration needed !
Fedora22_Minimal.img                                Fedora 22 minimal image (without Desktop)
Fedora22_Mate.img                                    Fedora 22 full Mate Desktop
Fedora22_LXDE.img                                   Fedora 22 LXDE Desktop
Kali_2.0-Xfce.img                                       Kali Linux 2.0 with full XFCE Desktop
ArchLinux_Minimal.img                            Arch Linux basic image (without Desktop GUI)
OpenSUSE_Tumbleweed_JeOS.img        OpenSuse JeOS  minimal image (without Desktop GUI)
OpenSUSE_Tumbleweed_XFCE.img        OpenSuse with full XFCE Desktop
Gentoo_full_cli.img                                   Gento Linux, configured (network, ssh, ntp, gentoolkit, tmux, mc, btrfs-progs installed)
OPI_slackware_14.1.img                           Slackware Arm 14.1  minimal image (without Desktop GUI)


! user name orangepi, password for user and root orangepi !


All images are ready to be installed on any Orange Pi H3 board, tested on Orange PI 2, Orange PI PLUS, Orange PI PLUS2, Orange PI PC.

In case of any problem, please first check if you have the latest kernel, desktop scripts, etc.

Installation on SD Card

Download any of the available images (xz archive) from Mega or GoogleDrive
Download scriptbin_kernel.tar.gz, it contains the latest kernel (uImage) and script.bin
Unpack the archive.
Write the xxx.img file (disk image) to your SD Card
on Linux use dd command ( sudo dd if=image_name.img of=/dev/sdX bs=1M oflag=direct )
on Windows use disk image writing software such as Win32 Disk Imager
After writing the image, mount SD Card FAT partition (BOOT)
Copy uImage_OPI-2 or uImage_OPI-PLUS (depending on your board type) to uImage (for OPI-PC use uImage_OPI-2)
Copy one of the script.bin.OPI-XXXX (depending on your board type and desired monitor resolution) to script.bin
use uImage_OPI-XX and script.bin.OPI-XXXX from scriptbin_kernel.tar.gz if it is newer (probably it is).
Boot your Orange PI board from SD Card
After booting resize Linux partition to fit your SD Card size:
sudo fs_resize
Copy the Code

Reboot

You can find the more detailed instructions for begginers here (thanks @Thumos)


Installation on internal EMMC

Install the image on SD Card as described above
Boot your Orange PI board from SD Card
Run:
sudo install_to_emmc
Copy the Code

Power off the board.
Remove SD Card
Power on, the board will boot from EMMC
You don't have to resize SD Card before installation on EMMC if you don't plan to use it.
You can use btrfs option to format your emmc Linux partition as btrfs, it will be mounted with compress=lzo option and you can save up to 40% of emmc size.
sudo install_to_emmc btrfs
Copy the Code

Backup internal EMMC to SD Card

Boot your Orange PI board from EMMC without SD Card inserted
login
insert your SD Card
Run:
sudo install_to_sdcard [btrfs]
Copy the Code

Your emmc Linux installation will be transfered to SD Card
You can boot from that SD Card on another or the same OPI board
If booting on different OPI board, remember to copy the kernel (uImage) and script.bin for that board


Booting from USB drive:

You can also boot from USB drive partition.
The file named cmdline.txt must exist on BOOT (fat) partition on sd card or emmc.
The content of that file must be: root=/dev/sdXn, where /dev/sdXn is the USB drive partition (as visible from OPI) to which to boot (for example root=/dev/sda1).
The line which mounts / in /etc/fstab on USB partition Linux must point to the right partition.
You can use install_to_usb script to install Linux oun USB drive partition and automaticaly create right cmdline.txt and fstab.
If cmdline.txt does not exist, or USB drive partition is not accesible (USB drive not attached), system boots to /dev/mmcblk0p1 (sdcard if inserted, otherwise emmc, if available) 
Bootable SD Card or EMMC must be accesible when booting to USB, but it is not necessary that the second partition contains valid Linux fs, sd card can have only the 1st (fat) partition
You can have different Linux installations on different USB drive partitions, just edit the cmdline.txt to select to which to boot.
The latest uImage must be used.

Using install_to_usb script:

Use install_to_usb script to install Linux to USB drive partition. Can be used to backup your SDCard/EMMC installation.
sudo install_to_usb /dev/sdXn [btrfs]|[noformat]
Copy the Code

/dev/sdXn is the USB drive partition to which to install (for example /dev/sda1)
if the second parameter is btrfs, USB partition will be formated as btrfs, otherwise as ext4
if the second parameter is noformat, USB partition will not be formated, content of the partition will be updated (in case you have used install_to_usb to backup your sdcard/emmc before)
be careful not to select the wrong USB partition, it will be erased/updated!
If the script does not exist on your image, download desktop_scripts.tar.gz, unpack to /usr/local/bin.
You must have the new uImage version, with boot to usb enabled.


If you are using an older image:

Download scriptbin_kernel.tar.gz from Mega, unpack.
Copy uImage_OPI-2 or uImage_OPI-PLUS (depending on your board type) to uImage on SD Card FAT partition
Copy one of the script.bin.OPI-XXXX (depending on your board type and desired monitor resolution) to script.bin on SD Card FAT partition
Copy lib/modules/3.4.39 directory to SD Card Linux partition (delete old first)
Copy all files (without lib directory) to /boot directory on linux partition in case you planing to install to emmc.
Backup your old kernel, script.bin and lib/modules/3.4.39 in case something goes wrong.



Features:

boot0_sdcard.fex, u-boot.fex and kernel (uImage) created from sources
kernel built with many features enabled (btrfs, USB serial adapters, bluetooth, hdmi sound, nfsd ...)
CPU runs at 1.53GHz, termal management adjusted so that all 4 cores are active up to 100 °C
GPIO, i2c (TWI), SPI enabled
Linux fs created from scratch with debootstrap
mimimal image can be the base for server or desktop
framebuffer console works
serial (uart) console works
ssh installed, root login ower ssh enabled
initial filesystem size less then 500 MB
initial RAM usage less then 50 MB
some usefull console programs installed (mc, htop, tmux, ...)
you can install server componnents (tested apache2, php, firebird, webmin, ...)
you can install full desktop, tested LXDE, XFCE and Mate desktop (recommended)
scripts to install lxde, xfce or mate desktop included, run (with lubuntu option the script will install Lubuntu core package, so you can get real Lubuntu look):
sudo install_lxde_desktop [lubuntu]
sudo install_mate_desktop
sudo install_xfce_desktop
Copy the Code
script to install x2go server (install only after desktop is installed), run
sudo install_x2goserver
Copy the Code

Notes
user name orangepi, pass for user and root orangepi
to enable wifi connection from command line run: sudo nmcli -a d wifi connect and enter your wifi credentials
to always run at full speed install heatsink and fan !
How to enable AP mode, see this post.

Building the system

You can try to build Debian/Ubuntu for OrangePI yourself.
Clone my GitHub repository.
You will need running Ubuntu or Debian system (you can even run it on OrangePI).
Before running the script install debootstrap and qemu-user-static packages.
Read carefully and edit params.sh to adjust the parameters to your needs.
Run sudo create_image to create Ubuntu system. I recommend to build to local directory, then you can run image_from_dir to transfer the system to sd card or image.


GitHub

Kernel sources and Ubuntu/Debian building scripts are available on github.

OrangePi kernel sources
Ubuntu/Debian building scripts
