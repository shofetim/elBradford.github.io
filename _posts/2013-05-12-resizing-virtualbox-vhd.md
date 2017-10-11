---
id: 1578
title: Resizing VirtualBox VHD
author: bradford
layout: post
guid: http://theblawblog.com/?p=1578
permalink: /2013/resizing-virtualbox-vhd
categories:
  - Linux
  - Technology
---
When I first created my VM for developing, I made the VHD a dynamic hard drive, but limited it to just 30GB. I didn&#8217;t think that I could fill that much space just doing Eclipse and Android SDK stuff, but I was obviously wrong. I tried following a couple of blog posts to complete the following steps, which I reference at the bottom of this post.<!--more-->

## Step 1: Resize the VHD

I first tried resizing the VHD with the VBoxManage command modifyhd:

```
VBoxManage modifyhd [vhd or vdi file] --resize [size in MB]
```

When I tried that command from an elevated command prompt, I got the following error:

```
VBoxManage.exe: error: Failed to create the VirtualBox object!
VBoxManage.exe: error: Code CO_E_SERVER_EXEC_FAILURE (0x80080005) - Server execution failed (extended info not available)
VBoxManage.exe: error: Most likely, the VirtualBox COM server is not running or failed to start.
```

Running from an un-elevated command line fixed the issue, however after resizing the VHD in this way, **the process apparently corrupted my MBR. Thankfully I made a backup and was able to start again. ALWAYS CREATE A BACKUP.**

Finally, I gave up and created a new VHD and cloned it using the GParted bootable ISO (see below) and dd from the command line:

```
# dd if=/dev/sda of=/dev/sdb
```

This took about 10 minutes for a 20GB VHD (/dev/sda). Once I had them cloned, I resized the guest file system in GParted.

## Step 2: Resize the Guest File System

Using GParted, I resized the guest file system in Ubuntu to fill the maximum space available so my development platform can expand as it needs to.

First, I downloaded the <a href="http://gparted.sourceforge.net/download.php" target="_blank">GParted bootable ISO</a>. I mounted that in VirtualBox for the guest OS, and booted it up. Since I used that Live CD for the dd operation in step one, I was already there. From here, I simply expanded the partitions to fill the available space. <a href="http://blog.mwpreston.net/2012/06/22/expanding-a-linux-disk-with-gparted-and-getting-swap-out-of-the-way/" target="_blank">Here is a quick guide on resizing a drive with a swap partition</a>.

After rebooting with the new VHD, all my data is present and I have a lot of room to grow!

## Resources:

  * <a href="http://justintung.com/2011/01/06/resize-and-expand-a-virtualbox-hard-drive-and-media-made-easy/" target="_blank"><span style="line-height: 15px;">Resize and Expand a Virtualbox Hard Drive and Media in 4 Steps</span></a>
  * <a href="http://cjhaas.com/blog/2012/11/27/error-when-resizing-virtualbox-disk-on-windows/" target="_blank">Error when resizing VirtualBox disk on Windows</a>
  * <a href="http://www.jim-doyle.com/?p=2012" target="_blank">Expand a VirtualBox .VHD with Damn Small Linux</a>
  * <a href="http://blog.mwpreston.net/2012/06/22/expanding-a-linux-disk-with-gparted-and-getting-swap-out-of-the-way/" target="_blank">Expanding a Linux disk with gparted (and getting swap out of the way)</a>