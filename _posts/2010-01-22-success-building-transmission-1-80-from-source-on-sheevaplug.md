---
id: 505
title: Success! Building Transmission 1.80 From Source on SheevaPlug
author: bradford
layout: post
guid: /success-building-transmission-1-80-from-source-on-sheevaplug/
permalink: /2010/success-building-transmission-1-80-from-source-on-sheevaplug
categories:
  - Sheevaplug
  - Technology
---
I saw the announcement on the Transmission website of the release of transmission 1.80 and I got really excited about some of the features such as tracker-less torrents and magnet links.  I was running 1.51 which is what’s in the Ubuntu Jaunty repos.  Unfortunately I can’t upgrade my SheevaPlug to any Ubuntu distro beyond 9.04 because [there is some funny business on how they’re compiling the arm packages for future releases][1]. Eventually I’ll switch over to Debian but I really don’t feel like mucking around with the serial console on my plug.<!--more-->

I have been avoiding having to build from source on my plug because I just didn’t want to figure it all out. But I found a <a href="http://trac.transmissionbt.com/wiki/Building" target="_blank">great guide</a> and <a href="http://forum.transmissionbt.com/viewtopic.php?f=2&t=9086&start=15#p42621" target="_blank">a post</a> that was fantastic and very easy to follow.

<pre class="brush:shell"># apt-get install build-essential automake autoconf libtool pkg-config libcurl4-openssl-dev intltool libxml2-dev libgtk2.0-dev libnotify-dev libglib2.0-dev</pre>

This command took about 10 minutes and around 160MB to install.

I then downloaded the latest release from <a href="http://download.m0k.org/transmission/files/" target="_blank">this website</a> (I chose the tar.bz archive)

To extract I simply typed in the following command:

<pre class="brush:shell"># tar –xvf transmission-1.80.tar.bz2</pre>

I entered the extracted directory and ran this command:

<pre class="brush:shell"># ./configure –disable-gtk</pre>

The –disable-gtk tag will disable the gui interface and make it compile faster.

After a while you should get a successful build

Before I ran the following command I backed up my transmission config file (/etc/transmission-daemon/settings.json) and purged transmission-daemon

<pre class="brush:shell"># aptitude purge transmission-daemon</pre>

I then proceeded with the build

<pre class="brush:shell"># make
# make install</pre>

Once this completed (it took around 30 minutes for me give or take) I could see that transmissioncli and transmission-remote existed in /usr/bin however I couldn’t find transmission-daemon. After a reboot it showed up.

Transmission 1.80 works! But I had to tweak it a bit to get it to work the way I wanted it to. I created an init.d script using <a href="http://trac.transmissionbt.com/wiki/Scripts/initd" target="_blank">these instructions</a>.  As the instructions suggested I created a new user named transmission with no password so that when I used ran the /etc/init.d/transmission-daemon script it would run as transmission and not as root. In addition, since transmission’s config files were in root’s home directory (~/.config/transmission-daemon/), I changed the TRANSMISSION_HOME variable in the script to /etc/transmission-daemon and copied all the files over and changed their ownership to transmission:transmission so transmission-daemon can see its config files and they would be easily accessible later.

The last thing I needed to do was update my settings.json from the backup I made before installing. In the process of doing this I noticed that watch directories are supported in this version of transmission-daemon! I’ve been using my own implementation of a watch directory as discussed <a href="/use-transmission-cli-to-download-from-rss/" target="_blank">in this blog post</a>, however having one built into transmission is much better in my opinion. From my reading, it looks like watch directories have been available for a while but just not for transmission-daemon.  To implement this first shut down transmission-daemon (/etc/init.d/transmission-daemon stop) and edit settings.json.  Add the following lines:

<pre class="brush:shell">“watch-dir”: “/your/watch/dir”,
“watch-dir-enabled”: true,</pre>

When you restart it should work! If for some reason it doesn’t work you can start transmission-daemon with the –f flag to see what’s wrong.

The great thing about the watch-dir as implemented by transmission is that it uses inotify, meaning it doesn’t just run as a cron job (polling) but it waits for the folder to notify it if there is a change. So, adding the torrents to the watch dir result in them being immediately added to transmission. The problem with the watch directory however is that they simply stay in the watch directory after they’ve been added. I would much rather they be moved to another folder or even deleted once added.

To use transmission’s watch-dir I simply need to stop my cronjob script that would add the torrents from the watch-dir to transmission and make sure that flexget was dropping the torrents from the rss into the correct watch-dir. <a href="/use-transmission-cli-to-download-from-rss/" target="_blank">See my post to read more about that</a>.

It works beautifully! Transmission is really starting to mature – I don’t miss uTorrent anymore!

 [1]: http://plugcomputer.org/plugforum/index.php?topic=885.0