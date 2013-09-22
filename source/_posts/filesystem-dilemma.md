title: the filesystem dilemma
date: 2013-08-30 17:13:47
tags: filesystem, ext4, ntfs, tuxera, extfs, windows, linux, ubuntu
---

Fellow readers, that's one of the oldest problems in the world, more or less like the chicken and egg question... or schroedinger's cat survivability chance.. or so it seems! 

**DISCLAIMER**: this first blogpost came out really long, the problem is i worked on this quite few days ;-) 

What i do in my spare time is developing javascript client and server side broken software, and playing random videogames to keep me entertained :) 
Said that, i've to admit i've used for a long time only m$ operating systems, just because i didn't need the rest... while still being attracted by the open-source world. 

This changed since most of modern web servers work on linux systems (95%?!?), so to test my deployment systems a linux machine is heaven, having the possibility to open multiple consoles without going mad with tmux (altough it ROCKS!!), and i also like writing commands instead of using mouse to do everything... 

So i converted myself happily to Ubuntu, everything very cool... but i also f...in LOVE Steam! YES! I still thank Gabe and valve software for this in the odd nights of loneliness. While the platform has some linux games and definitely not-bad support, i don't think the X server was thinked to be a gaming powerhouse, or that any of the videocard manufacturers are doing good work developing linux drivers... so... yeah... eww.. Welcome back Windows! 

Here it comes... dual boot, grub, settings again.. everything works!

Nice things, would be cool to make a good use of my hard drives! 
My configuration is composed of:

A) Samsung 120Gb SSD - 64Gb for Ubuntu (very much!), 64Gb for windows8 (not so much, [read here][winusage])  
B) WD Green 3Tb hard drive - 2800Gb for the "shared partition", 8Gb for swap partition  
C) Samsung Green 2Tb hard drive - 100% space for the "shared partition", it Will be used on another PC since this hd is detachable.  

## First Thing First

Originally B-1 was a NTFS partition for historical reasons. While being read smoothly by Ubuntu, the write performance is particurarly bad (30~ mb/s instead of potential 120), but reading it's not much better either!

This performance is kind of important when i transfer data between B and C disks, i don't wanna die of aging while the progressbar is loading... nonetheless, this kind of performance completely ruins the manufactorers mechanical efforts producing hard drives. 

#### Backup

As a start i formatted disk 4 with ext4, seeing very nice performance (95-100~mb/s) i was thinking i was going the right way.  
I copied all the data from disk B to disk C, that *took a while* also because reading speed was halved by ntfs driver. 

## First Try - ext4

After formatting B-1 as ext4 and B-2 as swap, i copied all the data back top speed on Linux and i was excited to try it on my win8 installation! 

*Excitement didn't last much*

I was supposing that being an opensource filesystem it would have good support outside Linux Kernel... how wrong was I!! 

Ext support on Windows might be summed by some link i cannot find ATM, in short only 3 software promising to read/write ext partitions:

  * Ext2Fsd - last version released in 2011 from then the development has halted, sourceforge project page is haunted. 
  * Ext2IFS - last release dated back 2008, i won't even try muttering with it for my own sanity.
  * Paragon ExtFS - recent release from Paragon software, while i like this company for their freeware/commercial products, it seems this project is closed source!

So, my 3tb drive was formatted with a GPT partition table (MBR doesn't let you use any more than 2048GB), using Paragon ExtFS it read my ext4 partition as Basic Disk (windows) so it didn't let me mount it. 
I've found a solution to this after N-hours debugging: you have to format your drive from Windows (as GPT), it creates some empty partitions (200mb at the start of the drive and 5mb at the end), then reboot into Linux and format B-1 as ext4. It will be read as ext4, still Paragon ExtFS crashes when mounting, **FAIL**.

My second (and last!) solution was Ext2Fsd, while it supports successfully ext2, it's clearly written on the website that Journaling is not supported, will probably break your data and it's not recommended to be used.
While my data isn't really **important**, I'm looking for a solution for everyday use, so i don't want my partition breaking to pieces after a week of use. 

#### First try outcome

I've tried anyway mounting the ext4 partition, reads successfully, also writes, but Ext2Fsd doesn't remember settings for ext4 partitions, so you:

  * can't use pagefile.sys, since at start the partition will be always read-only
  * will fear for your data to vanish someday... *soon*©

**NOTE:** speeds were everything excepts amazing on windows... (10MB/s write, 60-70MB/s read)

## Second Try - ext2 with Options

Well i was not satisfied, so i went back to Ubuntu, started formatting B-1 as ext2,and since it wasn't very fast i went for dinner. 

**HALF AN HOUR LATER**

Coming back i found gparted still misteriously formatting my partition, i pinched myself for a reality-check, well.. nothing happened, that was actually true! 

I stopped the process thinking that it hanged up at a certain point, rebooting and then running it from console i noticed the ext2 format takes a while writing superblocks.
That because ext2 was thinked for really small filesystems, a more-modern approach, like ext4, has an option for `lazy istantiation`; meaning the initial format is very fast, and when the partition gets mounted it quietly instantiates the superblocks.

So i tried to create an ext2 partition with ONLY these options activated (`uninit_bg` and `lazy_itable_init` for the nerds), it should be ext2-compatible? **No!** but i really didn't want to waste my life staring at the progress bar...

#### Second try outcome

I copied over my steam folder, from linux, at decent _- while not optimum -_ transfer speeds, rebooted into windows and started playing.

Don't Starve crashed after 15 minutes, and with him, the entire Ext driver crashed :) since the system became unresponsive and i had to shutdown the pc via plug-out!

**COOL EXPERIENCE, LET'S NOT REPEAT IT**

## Third Try - ext2

I'm really... really... unrelenting sometimes.

Probably it was ext4 fast format options that fucked up everything, let's make it slow and easy.  
So one `mke2fs /dev/sdb1` later, and another 80Gb Steam folder copy, i rebooted into windows.

#### Third try outcome

This came out depressing, i had the same problem as 2nd try, so driver crash while playing, Windows hangs up, plugoff.

*Eehhw... crap, Ext2Fsd seems really.. not adeguate.*

## The twist - HFS+

I went back to work and while explaining all that effort trying to *simply* use decently a partition with 2 operating systems (Ehy not 44 operating systems, just **TWO**), a friend of mine came out with the idea of thinking out of the box.

Thinking out of the box here means: why not use Apple stuff?

##### WAT ?!?

Okey i probably would need to wash multiple times before feeling myself clean again, but.. it could be worth trying.  
I formatted B-1 as HFS+. Linux support seems adeguate, except for the journaling, you **cannot** write on a HFS+ Journaled filesystem; too bad, who cares... it's not system-critical data anyway.

Writing performance from linux went well, i was pleased, it was similar to ext4, while CPU usage was of course much higher due to being a FUSE driver.

#### Fourth try not-outcome

I rebooted into Windows and i found out that... Windows doesn't support HFS at... **ALL**.  
Well, easy to guess the 2 companies behind those technologies doesn't love each other, but they have commercial contracts to support eachother's devices, for example an iPhone on Windows is automatically detected without any effort.  

But, **of course**, this doesn't apply to partitions, the fabric of your informations!  
I googled and googled, and seems there are different HFS drivers for Windows, that's where i found informations regarding the 5th try.

Anyway i wasn't really incline to buy a 20-30USD license to try out a partition that is not native to Any of my Operating Systems; but **probably** this is the right way if you are willing to spend some dollars, because:

  * a Windows commercial product probably works well, performing and being stable.
  * the Linux implementation seems really robust, i was positively impressed!

I wanted to try more _(4 tries and you want to try more? WTF)_ before give up to Apple's favourite filesystem!

### Fifth try - NTFS

[Introduction to status of NTFS in Linux.][linuxntfsstate]

Reading on tuxera and Paragon websites i discovered that NTFS technology is well used on corporate platforms on Linux, just normal human beings don't have access to those solutions.  
Earth is always a nice place to be. :-/  

Anyway, [NTFS "performance" driver from Tuxera][ntfstuxera] isn't available to single users, it is probably the most performing driver, from what [Tuxera writes][tuxeraperformance].
There is also a [Paragon product][ntfsparagon], but... seems really old (kernel 2.6.x ?!!), and costs only 40USD.

I would like to buy the paragon product if the informations were satisfactory, like.. my 3.8 kernel is supported or .. not?

I tried the tuxera free (former ntfs-3g) driver, it works fine with a modern kernel, but it gets outperformed by standard Linux FUSE implementation. (that is the same driver, just newer, probably!) 

So, kind of a _No_ for the **free** solution, a **maybe** for the commercial one.

#### Fifth try outcome

The only real handy thing that comes out the `ntfs-3g` package is the mkntfs tool, so i don't have to reboot into windows to format :) (or just install `ntfsprogs`)

Unless don't even try `ntfs-3g`, just use classic ntfs implementation, performance is is kind of 50% than native, when you're lucky, on Linux.

### Sixth try - exFAT

So i surrendered to the FUSE system, there's no way out of this after 5 tries, so i wanted to try the most common and (in) famous filesystem.  
I started meddling with Linux support, it doesn't seem bad, i've tried with very simple benchmark _(dd big file)_ and it steadily goes faster than NTFS & HFS+. Of course that doesn't mean much: usually videogames or, generally speaking, software are made of many small files so performance will degrade much, and usually more complex filesystems like the mentioned above manage that case **Much** better. 

Said that, formatting the partition with 8192 ` sectors-per-cluster` seemed to me the best-effort in performance, i reached a solid 150mb/s writing sequential files, never seen that number before! 

Linux FUSE for exFAT is not bad after all, the driver uses 2025% cpu usage for _One_ processor, has the chkfs, mkfs commands and supports all the capabilities of the filesystem (altough not being many, lol!)

#### Sixth try outcome

Of course windows stability and performance on this filesystem is optimal so...  
...i finally settled for the old dear FAT in 2013!

I don't find that as a disappointment, after all those tries i really suppose i should have tried before lady FAT. 

**Important update**: as i was writing this article i found out that came out public a [proprietary exFAT leak as kernel driver][nofuse]! You can read some funny background [here][funnybackground], aside from that, while for patenting reason this won't see the light in [official Kernel project][kernel], it might become a very stable solution for the private user who really isn't much concerned about license troubles. 

  [linuxntfsstate]: http://superuser.com/questions/139452/kernel-ntfs-driver-vs-ntfs-3g
  [winusage]: http://superuser.com/questions/463183/windows-8-disk-space-usage-vs-windows-7
  [ntfstuxera]: http://www.tuxera.com/mac/tuxera_ntfs_vs_ntfs-3g.pdf
  [tuxeraperformance]: http://www.phoronix.com/scan.php?page=news_item&px=OTU5Ng
  [ntfsparagon]: http://www.paragon-software.com/home/ntfs-linux-professional/comparison.html
  [kernel]: https://www.kernel.org/
  [nofuse]: https://github.com/dorimanx/exfat-nofuse
  [funnybackground]: http://phoronix.com/forums/showthread.php?81642-Native-Linux-Kernel-Module-Is-Out-For-Microsoft-exFAT&p=339638#post339638

