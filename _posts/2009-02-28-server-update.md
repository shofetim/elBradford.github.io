---
id: 165
title: Server Update!
author: bradford
layout: post
guid: /2009/02/28/server-update/
permalink: /2009/server-update
categories:
  - Home Server
  - Linux
---
In my post [Ch-ch-changes!][1] I spoke about getting WordPress to run on IIS 7, Vista, etc. It was a huge pain. 2 days ago I had a genius idea that I was surprised I hadn’t thought of before. What prompted it was <a href="http://www.engadget.com/2009/02/24/marvells-sheevaplug-linux-pc-fits-in-its-power-adapter/" target="_blank">an article on Engadget</a> about a tiny linux computer that would fit in a <a href="http://en.wikipedia.org/wiki/Wall_wart" target="_blank">wall wart</a> – TINY! It has a relatively powerful ARM processor (usually used in cell phones) with 512 MB RAM and 512 MB onboard flash memory. The first thing I thought about that was: <a href="http://en.wikipedia.org/wiki/LAMP_(software_bundle)" target="_blank">LAMP server</a>! I have to wait until the price drops to $50 to see if it will work.

This helped me realize that instead of messing with IIS and Vista, why not make a virtual machine with 512MB RAM and just a little processing power to see how it works? And so I downloaded VMWare server, Debian Lenny (debian compiles their distros for arm processors), and for the past 2 days I’ve been re-learning linux. Finally, it works and it works beautifully. Surprisingly MUCH faster than IIS 7. Although WordPress worked great on IIS 7, WordPress just wasn’t designed for it.

 [1]: /2008/12/24/ch-ch-changes/ "Ch-ch-changes!"