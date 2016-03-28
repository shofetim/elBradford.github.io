---
id: 269
title: SheevaPlug Unboxing and Ubuntu Install
author: bradford
layout: post
guid: /?p=269
permalink: /2009/sheevaplug-unboxing-and-ubuntu-install
categories:
  - Sheevaplug
---
It finally arrived! I woke up last week to find that there was a small box waiting for me to open.

<img class="aligncenter size-full wp-image-2425" src="https://bradford.la/wp-content/uploads/2009/04/f19161728.jpg" alt="f19161728" width="339" height="232" /><!--more-->

<img class="aligncenter size-full wp-image-2423" src="https://bradford.la/wp-content/uploads/2009/04/f19128968.jpg" alt="f19128968" width="300" height="225" /> <img class="aligncenter size-full wp-image-2422" src="https://bradford.la/wp-content/uploads/2009/04/f19115648.jpg" alt="f19115648" width="300" height="238" />

<p style="text-align: center;">
  <p style="text-align: center;">
    <p style="text-align: center;">
      <p style="text-align: center;">
        <p style="text-align: center;">
          <p style="text-align: left;">
            It was packaged surprisingly well. I&#8217;ve never purchased a dev kit before, but this was packaged better than any retail gadget I&#8217;ve purchased. Very impressed.
          </p>
          
          <h3>
            Installing Ubuntu
          </h3>
          
          <p>
            <a href="/a-17340-toothbrush-and-more-server-shenanigans/" target="_blank">I originally wanted to install Gentoo</a>, however I really didn’t want to go through the work of learning how to compile for this device,especially considering Marvel has already included an <a href="http://plugcomputer.org/index.php/us/resources/downloads" target="_blank">Ubuntu image</a> already optimized for the device.
          </p>
          
          <p>
            Install was a pain, only because I didn’t realize that the plug was already set up with Ubuntu installed! I made a few stupid moves and found myself having to start over.
          </p>
          
          <p>
            The Marvel documentation is a good place to start if you’re trying to install Ubuntu on the SheevaPlug. All the files and documentation you need are found <a href="http://plugcomputer.org/index.php/us/resources/downloads" target="_blank">on the SheevaPlug Dev Kit page</a>.
          </p>
          
          <p>
            Installing Ubuntu consisted of using the USB serial connection (I used <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/" target="_blank">PuTTY</a>, be sure to install the USB driver as directed in the documentation and connect at 115200) to talk to the Plug, installing a tftp and nfs server on my linux image, telling the Plug to go to the tftp server to grab the image that would allow it to boot from nfs and finally allow me to install the Ubuntu image onto the device. It really took me hours, since I ran into so many stupid issues caused by my ignorance of using tftp and nfs.
          </p>
          
          <p>
            I made the silly mistake of doing a dist-upgrade through aptitude. I eventually installed the Ubuntu image all over again because this made the device really unstable.
          </p>
          
          <p>
            It appears that when you boot off NAND, the OS is loaded into RAM as a ramdisk. That’s why some directories (like /var/apt/cache) are cleared after a reboot. Because of this, I followed the <a href="http://www.openplug.org/plugwiki/index.php/Frequently_Asked_Questions#Make_an_SD_card_be_the_root_filesystem" target="_blank">openplug.org walkthrough on installing the OS onto my 4GB class 6 SDHC card</a>. The result: bootup is much faster along with generally better performance all around.
          </p>
          
          <p>
            My next post will be about setting up the Plug as my LAMP server (well, now it’s my LLMP server).
          </p>