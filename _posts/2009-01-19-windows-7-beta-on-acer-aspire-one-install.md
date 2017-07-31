---
id: 119
title: Windows 7 Beta on Acer Aspire One Install
author: bradford
layout: post
guid: /2009/01/19/windows-7-beta-on-acer-aspire-one-install/
permalink: /2009/windows-7-beta-on-acer-aspire-one-install
categories:
  - Technology
tags:
  - Acer Aspire One
---
I got the Beta of Windows 7 the other day, and after hearing reports that <a href="http://weblogs.asp.net/guybarrette/archive/2009/01/09/windows-7-beta-acerone-performance-omg.aspx" target="_blank">Windows 7 is more netbook friendly</a>, I decided to try it out. I’m installing it on my Aspire One <a href="/acer-aspire-one-bluetooth-mod-soldering-info-ram-upgrade/" target="_blank">I’ve modded to 1.5GB RAM and bluetooth.</a>

<img class="aligncenter size-full wp-image-2427" src="https://bradford.la/wp-content/uploads/2009/01/f19139072.jpg" alt="f19139072" width="300" height="225" /><!--more-->

**Step 0: Obtain the Software** <a href="http://www.microsoft.com/windows/windows-7/beta-download.aspx" target="_blank">Get Windows 7 Beta here</a>. It may not be available for long, and if that’s the case and you are familiar with torrents, you can try downloading it from isohunt.com. Once you have the .iso file and a key (all the torrents I found had a key you can use to activate), you’re ready to continue. Try searching on btdig.com

**Step 1: Preparation **Since the open Beta is scheduled to expire some time in August 2009, and since there is a chance I’ll have problems after formatting my system partition, I created a disk image of my install using <a href="http://www.oo-software.com/home/en/products/oodiskimage/" target="_blank">O&O Disk Image Professional</a>. If you’re like me and find yourself reinstalling your OS nearly every month, then creating an image is vital – otherwise you will run out of activations on your MS licenses. My image was only 10GB and I store it on my desktop.

**Step 2: Install Media** Since the Aspire One doesn’t have a DVD drive, you can use either an external DVD drive or a USB drive. I followed the <a href="http://garyshortblog.wordpress.com/2009/01/10/how-to-install-windows-7-beta-on-an-acer-aspire-one-netbook/" target="_blank">instructions posted on Gary Short’s blog</a> on creating a bootable usb drive, however it didn’t work on my USB HDD (I’m not sure why). I then followed the instructions posted on <a href="http://www.istartedsomething.com/20081104/tip-make-your-pdc-2008-usb-hard-drive-a-bootable-windows-7-install-disk/" target="_blank">istartedsomething.com</a> to get my HDD bootable – I opened the Windows 7 .iso file in <a href="http://www.rarlabs.com" target="_blank">WinRar</a> and extracted the files to the partition created from this guide.

**Step 3: Install** I noticed that a 200MB “System Partition” was created during the install. <a href="http://www.sevenforums.com/general-discussion/1290-200-meg-hidden-system-partition.html" target="_blank">This thread at sevenforums.com explains why.</a> The install rebooted after a while and eventually loaded the User Setup screen (18 minutes into install) and finally loaded the desktop. **23 minutes total. WOW! That is very impressive.**

**Step 4:** **Drivers** Most drivers were installed automatically, including the Realtek NIC. After a reboot, the WLAN drivers were also installed. I’m not sure why they weren’t present on the initial boot. The only device that did not have drivers that installed automatically was the JMicron Card Reader. I tracked down the driver on their website and is available [here][1]. There were 3 unknown devices that were part of the card reader, so just manually update them and point them to the JMicron driver folder.

AMAZINGLY the webcam works with Microsoft’s generic drivers. I couldn’t get that to work in Vista.

(Update: New <a href="http://www.croftophile.fr/pilote/atheros_v7.6.1.194.exe" target="_blank">wireless drivers</a> [resolving LED issue], <a href="http://www.laptopvideo2go.com/forum/index.php?showtopic=21147" target="_blank">chipset drivers</a>, and <a href="http://www.laptopvideo2go.com/forum/index.php?showtopic=15103" target="_blank">touchpad drivers</a> are available <a href="http://www.aspireoneuser.com/forum/viewtopic.php?f=80&t=10036#p64655" target="_blank">from this thread</a>)

(Update: Build 7077 will download the latest Atheros drivers for the WiFi card from Windows Update. It works better than any other drivers I&#8217;ve tried. Very fast)

**Step 5: Software** The must-have software that I use regularly:

  * Firefox: Version 3.0.5 works perfectly, including my favorite plugins like Foxmarks, AdBlock Plus and Download Statusbar
  * MS Office 2007: Works.
  * Rocketdock: Works.
  * TuneUp Utilities 2009: Works. During the install there were several errors. I just ignored them, and TU 2009 seems to be running perfectly.
  * NOD32 2.7: Works.
  * WinRar 3.8: Works.
  * Virtual CloneDrive: Works.

**Step 6: Optimization** On every install of Vista I perform now, I go through <a href="http://www.tweakhound.com/vista/tweakguide/index.htm" target="_blank">TweakHound’s guide to optimizing Vista</a>. Some of the steps I performed were the same, such as disabling some of the visual effects, creating a documents partition etc. In addition, since TuneUp Utilities 2009 worked, and does such a great job at cleaning up and optimizing Windows, I used that as well. The end result is surprisingly fast – actually much faster than Windows Vista. Unfortunately, playing 720p h.264 compressed videos still struggles, but it seems to struggle less than Vista did.

**Final Impresions** I am very pleased with Windows 7  &#8211; the new task bar took some work getting used to, but only about 5 minutes worth for me. I removed all the quick launch buttons, as they confused me with running tasks (I would click on one to see why it was running, and it would open a new instance of the program). I also made the task bar auto-hide, something I’ve never done before but felt would be a good idea on the Aspire One since the vertical screen space is so limited.

Lastly, I don’t think I could justify paying ~$200 for Windows 7 as it stands now. With all the improvements, I don’t think it warrants a full release. Of course, I think it should be released as SP2, but even an inexpensive upgrade would be OK.

If you’re installing Windows 7 on *your* Aspire One, good luck. I hope the experience was as smooth as mine.

<img class="aligncenter size-full wp-image-2441" src="https://bradford.la/wp-content/uploads/2009/01/f19224200.jpg" alt="f19224200" width="300" height="246" />

**Update: 1/30/2009**

The following issues have come up:

  * WLAN light doesn&#8217;t work. When WLAN is turned off, the computer still thinks it has a wireless adapter active, but it won&#8217;t find any access points. It&#8217;s tricky to troubleshoot since the lights don&#8217;t indicate anything, but if you&#8217;re having issues connecting try toggling the WLAN switch and waiting a second.
  * Also, some explorer windows are blank. I am not sure what causes this problem. I can type in explicit directories and access them that way, but starting from Computer doesn&#8217;t work most of the time as nothing is displayed. It seems kind of random because it happens sometimes with Control Panel, or Windows Update. I haven&#8217;t seen anyone else with this issue however.

**Update: 2/4/2009**

The WLAN issue was resolved thanks to <a href="http://www.aspireoneuser.com/forum/viewtopic.php?f=38&t=10073&p=69826#p68308" target="_blank">a helpful poster</a> who pointed me to <a href="http://www.aspireoneuser.com/forum/viewtopic.php?f=80&t=10036#p64655" target="_blank">this thread</a> which also provided links to updated chipset drivers and touchpad drivers (allowing scrolling, etc). The updated chipset driver **increased** my Processor Subscore on the Windows Experience Index **from 2.1 to 2.3**. That&#8217;s great!!

The blank explorer windows issue seems to have disappeared after a reboot. If it comes back I&#8217;m going to file a bug report with Microsoft, though they make that kind of a hassle. Wait, it just came back. WTF&#8230;

**Update: 3/11/2009**

I got fed up the blank explorer window business and installed build 7022. It seems to work much better in that regard. There may be an even newer beta build than that, but 7022 works very well for me. If you want to get build 7022 I recommend <a href="http://btdig.com/windows%207%20build%207022" target="_blank">searching on btdig.com</a>.

**Update: 3/28/2009**

One day my AAO didn&#8217;t boot up. No POST, just a blinking hdd light and the sound of the fan turning on. I thought it was dead, but I browsed the internet and found [this amazing fix][2]. Apparantly it&#8217;s a widespread problem. I&#8217;m also planning on installing the latest Win 7 Beta once I get my server moved over to my sheevaplug. Currently the latest build is 7068.

**Update: 4/17/2009**

I downloaded and installed build 7077. It installed quickly, and the driver support seems better. The only thing I had to download manually was the JMicron driver for the memory card slots. The most recent version is [1.00.32][1].

 [1]: ftp://driver.jmicron.com.tw/jmb38x/XP_Vista_Win7/JMB38X_WinDrv_R1.00.32_WHQL.zip
 [2]: http://macles.blogspot.com/2008/08/acer-aspire-one-bios-recovery.html