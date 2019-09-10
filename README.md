# dual-boot-NetBSD-Linux
Dual Boot: The Unix BSD System, NetBSD, as main OS and Linux for rescue usage

## Install Linux with the Debootstrap Method

Make /dev/sda1 for maybe 500 Gb (here on this disk) with MBR 
+ create a rescue space /dev/sda2 for Linux (ext3) - 16Gb would be enough for CTWM and Xorg.

Once formated, mount /dev/sda2 /target and install Linux (Slack, Fedora, Devuan,...) using classical method.
The debootstrap for Devuan using deploy-devuan.sh is recommend there. 

## Install NetBSD on /dev/wd0 
Installer to install on WD0, edit MBR and install then the main NetBSD system on the space of 500 GB (here in this example). 

## NetBSD: Edit /etc/rc.conf + run services + wpa config for wifi (if needed)

pkg install ... as usual and then

create /grub with its config file as indicated below.

How to install GRUB2 on NetBSD
This how-to explains the steps needed to install GRUB2 on an existing i386/AMD64 NetBSD-installation. The steps should work on a properly chroot'ed system too. Tested on NetBSD 6.0 AMD64:

First of all, either download the package, using pkgin install grub2, or build it yourself from package sources (/usr/pkgsrc/sysutils/grub2).

After that, generate a GRUB configuration file, which tells GRUB the positions of the operating system(s). The following command will generate such a file, while adding your NetBSD system into it's list.

# grub-mkconfig -o /grub/grub.cfg
Now, install GRUB into your hard drive's master boot record (MBR). You have to know it's device name for this step (e.g. /dev/rwd0a). Exchange /dev/rwd0a with your desired device name, then change /dev/rXXXa to /dev/rwXXXd to access the raw disk, as in the following example:

Device name :   /dev/rwd0a
Direct access : /dev/rwd0d
The appropriate grub-install command for this drive would be:

# grub-install --no-floppy /dev/rwd0d
Hopefully, it should return : Installation finished. No error reported. If it does so, simply reboot the system and you should be greeted by a nice OS-selector. If not, recheck your device names, if that doesn't fix it, search the web for the error message. Even though there aren't that much resources about GRUB on NetBSD, you'll find a lot of information at GNU/Linux-related sites which apply to this scenario as well.

