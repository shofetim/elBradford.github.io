---
id: 1429
title: Restoring a Pharma Hacked WordPress Site (WP 3.4)
author: bradford
layout: post
guid: http://theblawblog.wordpress.com/?p=1429
permalink: /2012/restoring-a-pharma-hacked-wordpress-site-wp-3-4
jabber_published:
  - 1340316284
tagazine-media:
  - 'a:7:{s:7:"primary";s:58:"http://theblawblog.files.wordpress.com/2012/06/capture.jpg";s:6:"images";a:1:{s:58:"http://theblawblog.files.wordpress.com/2012/06/capture.jpg";a:6:{s:8:"file_url";s:58:"http://theblawblog.files.wordpress.com/2012/06/capture.jpg";s:5:"width";s:3:"844";s:6:"height";s:3:"404";s:4:"type";s:5:"image";s:4:"area";s:6:"340976";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"6182409";s:7:"blog_id";s:7:"9586444";s:9:"mod_stamp";s:19:"2012-06-21 22:13:27";}'
categories:
  - Security
  - Technology
  - Web Design
tags:
  - class-sftp.php
  - class-wp-error.php
  - legacy.php
  - native.php
  - pharma hack
  - string base64
  - wordfence
  - wordpress
---
I&#8217;ve been working on my <a href="http://charlesknutson.net" target="_blank">Professor K&#8217;s blog</a> and found that it had been hacked. It was kind of strange how I found that it was hacked; while trying to install a new plugin through the backend, the plugin window that pops up was filled with weird pharmacy links and images. I thought that was strange, so I searched the ends of the internet for answers. It is apparently a well-known and common exploit called the <a href="http://blog.sucuri.net/2010/07/understanding-and-cleaning-the-pharma-hack-on-wordpress.html" target="_blank">Pharma Hack</a>.  One of the best ways to see if you have been hacked is to run the <a href="http://sitecheck.sucuri.net/scanner/" target="_blank">Sucuri Sitecheck scan</a> on it. It&#8217;s free and will show you what pages are currently hacked.<!--more-->

There are lots of how-tos and guides online, but most of them are outdated. The pharma hack has evolved (<a title="How To Completely Clean Your Hacked WordPress Installation" href="http://smackdown.blogsblogsblogs.com/2008/06/24/how-to-completely-clean-your-hacked-wordpress-installation/" target="_blank">1</a>,<a title="How to find a backdoor in a hacked WordPress" href="http://ottodestruct.com/blog/2009/hacked-wordpress-backdoors/" target="_blank">2</a>,<a title="Decoding the Unusual WordPress &quot;Pharma Hack&quot;" href="http://www.science20.com/code_sorcery/blog/decoding_unusual_wordpress_pharma_hack-74914" target="_blank">3</a>).

I first tried to fix the issue by searching my wordpress directory for offending files, usually containing the string &#8220;base64_decode&#8221;. I used the following command in the wordpress directory

<pre style="padding-left: 30px;">$ grep -o -H -n -r "base64_decode" .</pre>

There were several files that did not appear to be real wordpress files, so I completely replaced the wp-includes directory with the code from the latest WordPress release.  This didn&#8217;t help.

I installed the <a href="http://wordpress.org/extend/plugins/wordfence/" target="_blank">Wordfence plugin</a>. It&#8217;s a fantastic plugin and does well at finding malicious files, however it appears that the pharma hack removes the plugin entirely! It is still very helpful at finding affected files.

After the malicious files kept coming back, I continued by <a href="http://codex.wordpress.org/Hardening_WordPress" target="_blank">hardening my wordpress installation</a>. The hack returned.

Desperate, I completely reinstalled the website from scratch, using the same database, copying back the uploads file (which contained no php code), reinstalling the plugins from wordpress directly, and using only the default themes plus my clean child theme.  The hack returned!

<p style="text-align: center;">
  <img class=" wp-image-1453 aligncenter" title="Wordfence alert" alt="" src="/wp-content/uploads/2012/06/capture1.jpg" />
</p>

Today I am looking at the files that Wordfence found either unrecognized, modified core files or malicious:

  * wp-admin/includes/class-sftp.php
  * wp-includes/native.php
  * wp-includes/class-wp-error.php
  * wp-includes/legacy.php

It looks like the modified core file, class-wp-error.php, is what the hackers use to bootstrap their exploit, because the only difference between the clean file and the compromised file is this single line:

<pre>/@require_once ('native.php');</pre>

native.php is an injected file and is full of obfuscated code.

The code is so obfuscated in fact that I can&#8217;t really follow how the other files relate, except that legacy.php seems to be what grabs the pharma code.

What I did is, in order,

  1. Restore class-wp-error.php and chmod it to 444 (not sure if that will help protect it but worth a shot)
  2. Deleted the other files Wordfence found
  3. Ran another Wordfence scan to make sure they&#8217;re all gone and clean
  4. Hardened my site again
  5. Removed FTP access and changed the SSH/SFTP password
  6. Installed <a href="http://wordpress.org/extend/plugins/better-wp-security/" target="_blank">Better WP Security</a> (<a href="http://wordpress.org/support/topic/plugin-wordfence-security-are-there-any-known-issues-with-better-wp-sercurity?replies=6" target="_blank">known to play well with Wordfence</a>)
  7. Reinstalled all my plugins (to be safe)
  8. Wait

I have a bad feeling that the site is still hacked and it&#8217;s only a matter of time before those malicious files appear again. I&#8217;ll update this if the site is still hacked.

**Update 6/25/2012: **

The site appears to be clean. [I&#8217;ve uploaded copies of the 4 files that were marked as malicious or unkown here][1]. It&#8217;s a .docx because wordpress.com limits the filetypes.

 [1]: /wp-content/uploads/2012/06/pharma-malicious-files.docx