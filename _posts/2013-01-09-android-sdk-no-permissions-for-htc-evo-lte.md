---
id: 1501
title: 'Android SDK: &#8220;no permissions&#8221; for HTC EVO LTE'
author: bradford
layout: post
guid: http://theblawblog.wordpress.com/?p=1501
permalink: /2013/android-sdk-no-permissions-for-htc-evo-lte
jabber_published:
  - 1357779347
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";i:0;s:6:"author";s:7:"6182409";s:7:"blog_id";s:7:"9586444";s:9:"mod_stamp";s:19:"2013-01-10 00:58:13";}'
categories:
  - Android
  - Technology
tags:
  - adb
  - Android SDK
  - HTC EVO LTE
---
I&#8217;m setting up a new dev environment in an Ubuntu VM. One of the toughest obstacles was getting adb to recognize my HTC EVO without starting adb as root. None of the posted solutions worked for me, but they were helpful, <a href="http://s.android.com/source/initializing.html" target="_blank">particularly the setup instructions posted by Google under &#8220;Configuring USB Access&#8221;</a>.  <!--more-->

I used the command

> <pre>$lsusb</pre>

and got the following response:

> <pre>Bus 001 Device 004: ID 0bb4:0ce9 HTC (High Tech Computer Corp.)</pre>

I used `0bb4:0ce9` obtained from the usb listing to modify the lines in `/etc/udev/rules.d/51-android.rules` to be:

```# adb protocol on jewel (EVO LTE)
SUBSYSTEM=="usb", ATTR{idVendor}=="0bb4", ATTR{idProduct}=="0ce9", MODE="0600", OWNER="bradford"
# fastboot protocol on jewel (EVO LTE)
SUBSYSTEM=="usb", ATTR{idVendor}=="0bb4", ATTR{idProduct}=="0ce9", MODE="0600", OWNER="bradford"
```

I can now access my usb device without root access.