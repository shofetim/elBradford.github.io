---
id: 283
title: Turning My SheevaPlug Into A Web Server
author: bradford
layout: post
guid: /2009/04/18/turning-my-sheevaplug-into-a-web-server/
permalink: /2009/turning-my-sheevaplug-into-a-web-server
categories:
  - Sheevaplug
---
After I got Ubuntu running on my Plug, I started going about turning it into <a href="/a-17340-toothbrush-and-more-server-shenanigans/" target="_blank">what I originally planned for it</a>; LAMP, UPnP, and Torrent server.

I first found a great resource for playing with the plug: <a href="http://openplug.org" target="_blank">OpenPlug.org</a>. The wiki in particular has some good tips, the first of which I followed below.

### Install Root FS on SDHC Card

<!--more-->

By default, the [SheevaPlug][1] has 512MB of NAND flash memory for storage, and 512MB or RAM. When you boot your plug from NAND it copies the necessary files to a RAM disk (a virtual disk on RAM which disappears when the plug is powered off). If I understand correctly, this will in effect reduce the available RAM since some of it is used by the file system.

By following <a href="http://www.openplug.org/plugwiki/index.php/Frequently_Asked_Questions#Make_an_SD_card_be_the_root_filesystem" target="_blank">the tutorial found in the OpenPlug.org wiki</a>, I was able to instead copy the filesystem to the SDHC card (I used a 4GB Sandisk Extreme III Class 6) and then boot from it.

Edit 5/22/2009: It appears they&#8217;ve edited the wiki, and while you can look at the history of the article I linked to, I&#8217;ll post the instructions here for posterity. Please follow the steps carefully.

### **Make an SD** card** be the root filesystem**

Prerequisites:

  * SD card ( 512mb or bigger )
  * working serial console
  * a working system with &#8216;cat /proc/mtd&#8217; showing a rootfs (for me it was mtd1)

Format your SD card to a filesystem that supports permissions (far as I know fat32 will not work, but I didn&#8217;t try) &#8211; I used &#8216;fdisk /dev/mmcblk0&#8217; and formatted the partition with ext3 filesystem &#8216;mkfs.ext3 /dev/mmcblk0p1&#8217;

Copy the existing root filesystem into the SD card. (assuming mtd1 is your rootfs device)

> <pre>*  mkdir /mnt/sd</pre>
> 
> <pre>*  mkdir /mnt/tmproot</pre>
> 
> <pre>*  mount /dev/mmcblk0p1 /mnt/sd</pre>
> 
> <pre>*  mount /dev/mtdblock1 /mnt/tmproot</pre>
> 
> <pre>*  cp -av /mnt/tmproot/* /mnt/sd</pre>
> 
> <pre>*  umount /mnt/tmproot</pre>

Update the SD cards fstab to mount itself as root:

> <pre>*  vim /mnt/sd/etc/fstab</pre>

change

<pre>rootfs / rootfs rw 0 0</pre>

to (assuming ext3 filesystem)

<pre>/dev/mmcblk0p1 / ext3 rw 0 0</pre>

> <pre>*  umount /mnt/sd</pre>
> 
> <pre>*  reboot</pre>

Get to the u-boot prompt (Marvell>>) &#8211; this can be done by having the serial console connected while rebooting the device, it gives you 3 second to hit any key, just hit a key.

Backup bootargs_root and bootargs settings.

> <pre>*  printenv bootargs_root</pre>

Mine: bootargs_root=root=/dev/mtdblock2 ro

> <pre>*  printenv bootargs</pre>

Mine: bootargs=console=ttyS0,115200  
mtdparts=nand_mtd:0x400000@0x100000(uImage),  
0x1fb00000@0x500000(rootfs)  
rw root=/dev/mtdblock1 rw ip=10.4.50.4:10.4.50.5  
:10.4.50.5:255.255.255.0:DB88FXX81:eth0:none

Change the root filesystem to the SD card.

> <pre>*  set bootargs_root 'root=/dev/mmcblk0p1'</pre>
> 
> <pre>*  set bootargs=console=ttyS0,115200 mtdparts=nand_mtd
:0x400000@0x100000(uImage),0x1fb00000@0x500000(rootfs)
rw root=/dev/mmcblk0p1 rw ip=10.4.50.4:10.4.50.5:10.4.50.5
:255.255.255.0:DB88FXX81:eth0:none</pre>
> 
> <pre>*  saveenv</pre>
> 
> <pre>*  reset</pre>

The output of &#8216;df -h&#8217; now shows (for my 8gb SD card):

> <pre>Filesystem            Size  Used Avail Use% Mounted on</pre>
> 
> <pre>/dev/mmcblk0p1        7.4G  487M  6.6G   7% /</pre>
> 
> <pre>tmpfs                 252M     0  252M   0% /lib/init/rw</pre>
> 
> <pre>varrun                252M   36K  252M   1% /var/run</pre>
> 
> <pre>varlock               252M     0  252M   0% /var/lock</pre>
> 
> <pre>udev                  252M   16K  252M   1% /dev</pre>
> 
> <pre>tmpfs                 252M     0  252M   0% /dev/shm</pre>

If something goes wrong and it does not work, you should be able to set the bootargs_root and bootargs back to what they were.

The performance increase was surprising and immediate. My plug booted I would guess 4 times as fast as before.

### Install Lighttpd, MySQL and PHP5 for WordPress and sub-domains

I was originally going to use Apache, but after hearing the performance benefits of lighttpd over apache, not needing the robustness of apache, and after verifying that lighttpd will work with wordpress, I jumped.

Instructions on how to do so are also <a href="http://www.openplug.org/plugwiki/index.php/Frequently_Asked_Questions#How_do_I_set_up_a_webserver_on_the_plug.3F_Can_I_use_php.3F" target="_blank">found on the OpenPlug.org wiki</a>.

Since I run two wordpress blogs off of my server, I made the following additions to my /etc/lighttpd/lighttpd.conf file

> <pre><span style="font-family: cOUR;">$HTTP["host"] == "bradford.la" {
 server.document-root = "/var/www/bradford.la"
 server.errorlog = "/var/log/bradford.la"
 accesslog.filename = "/var/log/bradford.la"
 dir-listing.activate = "disable" </span></pre>
> 
> <pre><span style="font-family: cOUR;">url.rewrite-once = (
 "^/(.*)?/?files/$" =&gt; "index.php",
 "^/(.*)?/?files/(.*)" =&gt; "wp-content/blogs.php?file=$2",
 "^/(wp-.*)$" =&gt; "$1",
 "^/([_0-9a-zA-Z-]+/)?(wp-.*)" =&gt; "$2",
 "^/([_0-9a-zA-Z-]+/)?(.*.php)$" =&gt; "$2",
 "." =&gt; "index.php"
 )
 } </span></pre>
> 
> <pre><span style="font-family: cOUR;">else $HTTP[host] == “…..</span></pre>

The first line defines one virtual server (in Apache terms). There are lines in there to set up <a href="http://www.cyberciti.biz/tips/howto-lighttpd-web-server-setting-up-virtual-hosting.html" target="_blank">logging for the virtual server</a>. The line “dir-listing.activate = disable” is important, since lighttpd will allow directory listing by default. The url.rewrite-once is necessary to have your wordpress blog <a href="http://snipplr.com/view.php?codeview&id=5979" target="_blank">work with url-rewriting</a>. After that, the last line of the above example shows how you can define further sites using <a href="http://zargony.com/2008/02/04/migrating-from-apache-to-lighttpd-with-name-based-virtual-hosts-and-ssl" target="_blank">else statements</a>.

Installing MySQL was as simple as

> <pre><span style="font-family: Courier;">apt-get install mysql-server mysql-client</span></pre>

and following the on-screen instructions for setting up your MySQL passwords.

I did toy with using sqlite for a more efficient server, however I couldn’t get it to work with wordpress. There is a <a href="http://wordpress.org/extend/plugins/pdo-for-wordpress/" target="_blank">wordpress plugin</a> for switching to sqlite, however it didn’t work with my setup. After <a href="http://chrisjohnston.org/2008/configuring-a-lightweight-apache-mysql-install-on-debian-ubuntu" target="_blank">optimizing mysql</a> the memory footprint was manageable.

I installed phpmyadmin, and imported my sql tables from my old server. Everything imported well, and after disabling and re-enabling my plugins my sites were up and running even better than before. I’m very impressed with the performance of LLMP (Linux, Lighttpd, MySQL, PHP) on my Plug.

I’ll cover installing a torrent client (I am thinking Deluge) and UPnP (MediaTomb?) in my next post.

 [1]: http://theblawblog.com/category/sheevaplug-project-technical