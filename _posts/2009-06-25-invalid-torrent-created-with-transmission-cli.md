---
id: 380
title: Invalid Torrent Created With transmission-cli
author: bradford
layout: post
guid: /2009/06/25/invalid-torrent-created-with-transmission-cli
permalink: /2009/invalid-torrent-created-with-transmission-cli
categories:
  - Home Server
---
I’m posting this mainly because I had a really hard time finding the solution. Hopefully someone in the same boat can find this solution helpful.

I usually use uTorrent to create torrents, however <a title="Turning My SheevaPlug Into A Web Server" href="/2009/04/18/turning-my-sheevaplug-into-a-web-server/" target="_blank">since I switched to my sheevaplug and installed transmission</a> I&#8217;ve had to learn how to manage my torrents with a very limited cli. I did learn how to create a torrent:<!--more-->

<pre>transmissioncli -n &lt;file to be shared&gt; -a &lt;URL of torrent announcement&gt; &lt;torrent file filename&gt;</pre>

The problem I ran into after creating the torrent and uploading it to my private tracker of choice (demonoid) and then downloading the posted torrent file, it would try to download the files. It would not see them as already existing in my torrent directory. I did some sleuthing and found that the first character of any file or folder in the directory of my torrent was cut off. For example, if in my transmission cli command I put

<pre>transmissioncli -n Ignotus/ -a &lt;URL of torrent announcement&gt; &lt;torrent file filename&gt;</pre>

it would successfully create a torrent for the directory Ignotus. However any of the files in that folder would be listed without the first character. So, if the files were tracks from the Ignotus album with leading zeros (01, 02, 03 etc) they would instead be (1, 2, 3). This made the torrent client not recognize that the files were already downloaded.

I sleuthed on the <a href="http://webchat.freenode.net/" target="_blank">transmission irc channel (#transmission)</a> and someone pointed me to the <a href="http://trac.transmissionbt.com/ticket/2227" target="_blank">following bug</a>. So, to solve this issue either download the latest nightly or simply do not use the trailing slash when specifying a directory. This might get hairy if you have files the same name as your directories but that probably won’t be a problem.

Update 7/28/2009: [Looks like this is fixed in 1.73.][1]

 [1]: http://trac.transmissionbt.com/query?milestone=1.73&group=component&groupdesc=1&order=severity