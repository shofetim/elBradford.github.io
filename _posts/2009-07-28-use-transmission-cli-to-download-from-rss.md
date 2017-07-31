---
id: 387
title: 'How To: Transmission-CLI and RSS'
author: bradford
layout: post
guid: /2009/07/28/use-transmission-cli-to-download-from-rss
permalink: /2009/use-transmission-cli-to-download-from-rss
categories:
  - Home Server
  - Linux
---
I really like Transmission-CLI for torrents, however it is lacking many features I used with uTorrent. One of them is downloading torrents automatically from an RSS feed. My reason for this is that I love Top Gear. But I don’t have BBC America (or a TV) so my only alternative is downloading it. But I don’t want to have to manually find the latest episode and then wait for it to download. This is a short how-to to get a simple RSS system set up using transmission-cli.<!--more-->

Prerequisites:

  * transmission-cli set up and running on your computer (on debian systems it’s as simple as “apt-get install transmission-cli”)

**Step 1:** **Find your torrent feeds**

I use <a href="http://showrss.karmorra.info" target="_blank">showRSS</a>. showRSS is great simply because it is easy to join and easy to create a custom feed of your favorite television shows. I have a feed listing any new episodes from Top Gear, Lost, Flight of the Conchords and Law & Order.

An alternative was posted as <a href="http://blog.karmorra.info/?p=79" target="_blank">a comment</a> on the showRSS blog and sounds like a good idea:

> Just go to mininova.org and do a search for the show you want. You’ll see an orange RSS feed button. You can copy the url from the button and paste it right into a Miro (Sidebar -> Add Feed). THe only hard part is you’ll want to narrow down the search first at Mininova, so you don’t get all sorts of crap. For example, eztv seems to be a big distributor, so you could try searching “true blood eztv” and you see a nice list gets narrowed down. If you only want the smaller files and not the 720p ones, you can refine it further with “true blood eztv -720p”. Now just right click the rss button and copy the url. Add a new feed to miro with this url, and you’re set! Sometimes you have to play around a bit in mininova to get a clean search for just the shows you want. Looking in the right column can help to see which shows have lots of seeds. Eztv is a good one to try by default but won’t be for every show.

**Step 2: Install FlexGet**

<a href="http://flexget.com/" target="_blank">FlexGet</a> will automatically download from your feed into a specified directory. The <a href="http://flexget.com/wiki/Install" target="_blank">install instructions</a> are very easy and straightforward. My configuration file is very simple:

<pre class="brush:shell">feeds:
    Top_Gear:
    rss: http://showrss.karmorra.info/rss.php?##your_feed##
    download: /new_torrents/</pre>

This step is in the install instructions for FlexGet but I’ll include it here since it’s important.

We want FlexGet to run at a particular interval – hourly in my case. So I type in the command

<pre class="brush:shell">$ sudo crontab -e</pre>

Then add this line to your crontab config file:

<pre class="brush:shell">@hourly ~/flexget/flexget.py -q</pre>

Now you have FlexGet checking your feed every hour and downloading new torrents to your download folder.

**Step 3: Tell transmission to download the new torrents**

Update 22 January 2010: The new version (1.80) of transmission-daemon supports watch directories natively – <a href="/success-building-transmission-1-80-from-source-on-sheevaplug/" target="_blank">see this post for more details</a>! **This step is probably unnecessary.**

Transmission-cli does not currently support watch folders. So we need a script to do the same thing. This is actually a very easy step. <a href="http://trac.transmissionbt.com/wiki/Scripts/Watchdog" target="_blank">There is already a script on transmission’s website</a> for this purpose. I did, however, have to tweak it for it to work on the newer version of transmission-cli.

Create a file called watchdog.sh. It can be anywhere, but I put mine in my FlexGet directory. Next, paste this code into the file:

<pre class="brush:shell">#!/bin/bash

# Watch dir, may contain spaces:
watchdir="/new_torrents/"

# move file to a subdirectory? if Commented out, it'll removed remove
# the torrent file.
# Note: Don't put a '/' before the path!
#movesubdir="added/"

# Authentication "username:password":
tr_auth="transmission:transmission"

# Transmission host "ip:port":
tr_host="127.0.0.1:9091"

# Verbose?
verbose=0

#############################################
time=$(date "+%Y-%m-%d (%H:%M:%S)")
if [ -n "$tr_auth" ]; then
tr_auth="-n $tr_auth"
fi

for file in "$watchdir"*.torrent
do
if [ -f "$file" ]; then
if [ -n "$verbose" ]; then echo "$time: $file added to queue."; fi

/usr/bin/transmission-remote "$tr_host" "$tr_auth" –a "$file" &gt; /dev/null

# give the remote some time to process
sleep 5

if [ $movesubdir ]; then
if [ -d "$watchdir$movesubdir" ]; then
mv "$file" "$watchdir$movesubdir"
else
mkdir "$watchdir$movesubdir"
mv "$file" "$watchdir$movesubdir"
fi
else
rm "$file"
fi
else
if [ -n "$verbose" ]; then echo "$time: No torrent in $watchdir."; fi
fi
done

exit 0</pre>

Next we need to make it executable:

<pre class="brush:shell">$ sudo chmod +x watchdog.sh</pre>

This should work. Just change the watchdir variable to the directory that FlexGet downloads your torrents to. If you have authentication set up for your transmission-daemon then be sure to plug your credentials into this script as well.

Finally we add another entry to crontab:

<pre class="brush:shell">@hourly ~/flexget/watchdog.sh</pre>

Make sure this entry has the correct path to your script.

**Conclusion:**

You should now have a working automatic TV downloading system. If you have any questions feel free to ask.

**Update** 16 October 2009: I&#8217;ve set this up again on a new install of Ubuntu using the latest versions of flexget, python, and transmission-cli available through aptitude and it still works well with the following exception: for some reason transmission-remote is not authenticating with the &#8220;<span style="font-family: courier new;">tr_auth</span>&#8221; variable. Instead I hardcoded my username:password into the /usr/bin/transmission-remote command and it works. I&#8217;ll update this if I figure out what&#8217;s wrong with using the &#8220;<span style="font-family: courier new;">tr_auth</span>&#8221; variable.

The new lines are modified to the following:

<pre class="brush:shell"># Authentication "username:password":
#tr_auth="transmission:transmission"

#if [ -n "$tr_auth" ]; then
#tr_auth="-n $tr_auth"
#fi

/usr/bin/transmission-remote "$tr_host" -n username:password –a "$file" &gt; /dev/null</pre>

&nbsp;

&nbsp;