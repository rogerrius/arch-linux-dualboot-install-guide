# Arch Linux dualboot install guide (October 2024)

Easiest and fastest way to install Arch Linux on your PC. 

## Installation

### Disk partitioning 
Once your Arch Linux ISO is ready to type commands do:

Open the CLI tool cfdisk to start partitioning the disk you want to install Arch:

    $ cfdisk /dev/{YourDiskName}

And create partitions:

    * EFI: 500M
    * LINUX FILESYSTEM: the desired space for Arch

Apply changes and exit cfdisk.

### Connect to internet wirelessly

    $ iwctl
        > device wlan0 set-property Powered on
        > station wlan0 scan
        > station wlan0 get-networks
        > station wlan0 connect "wifi_name"


### Archinstall script
    $ archinstall

Fill everything as desired and in the disk section select manual and mount the `/` in the thick partition (linux filesystem mentioned above) and mount `/boot` in the EFI partition.

Install and wait for it to finish and reboot.


### Post-install configurations

#### - LINUX HEADERS
When updating with `sudo pacman -Syu` if the initramfs gives errors when compiling, when updating pacman or when installing packages, you must install:

    $ sudo pacman -S linux-headers # For Stable kernel
    $ sudo pacman -S linux-zen-headers # For Zen kernel
    $ sudo pacman -S linux-hardened-headers # For Hardened kernel

#### - WIFI

    $ nmcli device wifi list    
    $ nmcli device wifi connect "wifi-name" password "passwordHere"




### Repair grub

This way you can see Windows installation on grub OS selector.

    $ sudo pacman -S os-prober
    $ sudo nano /etc/default/grub
        > Uncomment os-prober line
    $ sudo grub-mkconfig -o /boot/grub/grub.cfg
