title: grub4dos in 10 minutes
tags: grub, grub4dos, ubuntu, linux, 12.04, usbdrive, boot, usbpen
date: 2014-03-20 11:01:18
---
One of the main issues that slows down the adoption of grub4dos is the lack of recent documentation or examples. 

## For those that have no idea of what is it:

Is a boot loader with premium functionality, designed for ease of use, and usable with FAT32 and NTFS.  
Yeah for those that know GRUB, it might seem an aberration, and it might as well be! It's very handy to make multiboot devices,
especially usb pen drives (although it might be used as normal bootloader for your hard drive)

The `premium` functionality I'm talking about are: boot of _.iso_ images, loading them into ram, easy change drive composition
(ex: rename hd0 to hd4, if that might be useful), compatibility with old DOS images

## What i use it for

Recovery USB pen with ubuntu livecd (the really heavylifter here!), dos command line (just downloadable `.img` [from Internet](http://www.allbootdisks.com/download/dos.html) )
for firmware upgrades, wierd commercial recovery software (in case i ever need that...)

Such a drive can handle windows installations (more info on the next blogpost), ubuntu installations, data recovery from win/linux, drive resize and... many other things!

## Snippet for ubuntu 12.04

{% codeblock menu.lst lang:dos %}
title Ubuntu 12.04.4
find --set-root /ubuntu-12.04.4-desktop-amd64.iso
map /ubuntu-12.04.4-desktop-amd64.iso (hd32) || map --mem /ubuntu-12.04.4-desktop-amd64.iso (hd32)
map --hook
root (hd32)
kernel /casper/vmlinuz.efi file=/preseed/ubuntu.seed boot=casper persistent iso-scan/filename=/ubuntu-12.04.4-desktop-amd64.iso
initrd  /casper/initrd.lz
{% endcodeblock %}

Okey so how do you use that? Right.

1) Don't use vanilla grub4dos, that is OLD!
2) Download stable* 0.4.5 [chenall grub](https://code.google.com/p/grub4dos-chenall/downloads/list)
3) Download also the latest 0.4.6 release, it usually haves new features!

INSTALL TIME:

A) Unzip (2) `7z x grub4dos-0.4.5c-2014-01-17.7z`
B) Unzip (3) `7z x grub4dos-0.4.6a-2014-01-17.7z`
C) Run `grub4dos-0.4.5c/bootlace.com <DRIVE>`, this **WILL OVERWRITE THE MBR**
D) Mount your drive with commandline or via UI
E) Copy `cp grub4dos-0.4.6a/grldr grub4dos-0.4.6a/sample/menu.lst grub4dos-0.4.6a/sample/default <MOUNTPOINT>`
F) Customize `<MOUNTPOINT>/menu.lst` with your favorite iso/commands

Reference to [grub4dos documentation](http://diddy.boot-land.net/grub4dos/Grub4dos.htm), quite old but gold!

*: we are downloading 2 versions because 0.4.6 line gives me `Segmentation fault` when running `bootlace.com`!
