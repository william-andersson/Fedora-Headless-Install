# Fedora Linux Remote Headless Install (the easy way!)

## Intro
I have previously been using openSuse for my headless server and when time comes for installation they provide a fairly simple way to use ssh/vnc for the installer by using 'mksusecd' to modify the boot options.

```
SSH
sudo mksusecd --create suse-leap-ssh.iso --boot "ifcfg=*="dhcp" self_update=1 language=en_US ssh=1 ssh.password=PASSWORD" openSUSE-Leap-15.5-DVD-x86_64.iso

VNC
sudo mksusecd --create suse-leap-ssh.iso --boot "ifcfg=*="dhcp" self_update=1 language=en_US vnc=1 vncpassword=PASSWORD" openSUSE-Leap-15.5-DVD-x86_64.iso

```

Now I would like the same functionality for Fedora and have been searching the entire web for a simple solution.
After a week of trial and error trying both the kickstart method and modified iso's I finally found a way that is as simple or
maybe more so than the openSuse approach.

## Solution
It turns out that Fedora-Media-Writer creates a second partition on the usb named Anaconda wich contains a EFI directory with a BOOT directory inside that holds a grub.cfg you can modify. Adding 'inst.vnc' to the end of the boot parameters enables you to boot the installer with vnc support! You can also change default boot option and timeout.

```
linuxefi /images/pxeboot/vmlinuz inst.stage2=hd:LABEL=Fedora-S-dvd-x86_64-40 quiet inst.vnc
```

>[!NOTE]
>This only works with the server edition of Fedora, not workstation.

## Notes
>[!TIP]
>If you use UEFI and leave the usb in, you can when time comes use efibootmgr to set next boot from that usb to initiate the re-install remotely.
>
>```
>sudo efibootmgr (to find device numbers)
>sudo efibootmgr -n XXXX (replace XXXX with device number)
>sudo reboot
>```
