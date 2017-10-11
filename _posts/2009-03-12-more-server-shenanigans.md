---
id: 168
title: A $173.40 Toothbrush and More Server Shenanigans!
author: bradford
layout: post
guid: /?p=168
permalink: /2009/more-server-shenanigans
categories:
  - Sheevaplug
---
I went to the dentist today for the first time in over 3 years. I know, and I&#8217;m a bad flosser too. Since I don&#8217;t have dental insurance, the cost for the cleaning and x-rays was $173.40 which included a complimentary toothbrush. Why the toothbrush? Why not just knock $100 off my total bill? I would like that much more than a red toothbrush and some floss.

And now the server shenanigans! <!--more-->I decided today to get rid of my desktop. Not when I get my 

[SheevaPlug][1], but NOW (I have a hard time concentrating on homework when I could be playing [Team Fortress 2][2]). So, I took some pictures, slapped it on the internet, and took my Debian lamp server image from my desktop and put it on my Aspire One.  Yep! My little guy is now a server! It serves up cached pages from WordPress really quickly but some of the back end stuff (wp-admin) is a little slower.  The music in my other blog posts won&#8217;t work until I get the music from my desktop into an external hard drive. Things are looking good for that SheevaPlug. It has a slower clocked CPU, and instead of x86 it&#8217;s ARM, so I really don&#8217;t know what to expect. But at least I know it will work.

![](/assets/images/posts/archive/2009/03/f19369736.jpg)


I&#8217;ve actually ordered the SheevaPlug &#8211; it should ship sometime this month. Last night I also ordered a <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16817182144" target="_self">2-drive JBOD USB hard drive enclosure</a> (to put 2 hard drives in that you can just plug into a computer via USB) along with a big fatty 1TB HDD for all my media files for my current Aspire Server and my SheevaPlug server once I have it all set up.

I spoke with the uber-nerd at N.E.R.D Command Central Defcon Enforcement Datacenter (ie BYU CS System Admins) and he suggested I use Gentoo on my SheevaPlug because I can compile it all from source and have little overhead (ie unneeded stuff included in a basic Linux distro). To be honest I&#8217;ve never compiled an entire OS so this will be very educational. But I agree, I want as little overhead as possible. Once I get a good image I&#8217;ll probably post it for download for anyone who is interested.

This is what I plan to put on my SheevaPlug Server:

  * Gentoo Linux
  * **L**inux
  * **A**pache
  * **M**ySQL
  * **P**HP
  * [Subsonic Media Server][3]
  * [Firefly (DAAP) Server][4] (I don&#8217;t use iTunes though&#8230;)
  * [UPnP Server][5] (not sure which one to use&#8230; [uShare][6]? [MediaTomb][7]? [FUPPES][8]? [GMediaServer][9]? [VUZE][10]?) I need to find which is the most efficient.
  * BitTorrent &#8211; I love [uTorrent][11], however they don&#8217;t make a version for Linux(WHY??). VUZE would work for this and as a UPnP server, but I&#8217;ve heard bad things about the efficiency of Azureus (where VUZE came from).
  * SAMBA server

I&#8217;ll post about my progress once I get my SheevaPlug.

 [1]: /server-update/
 [2]: http://teamfortress.com
 [3]: http://subsonic.sourceforge.net/
 [4]: http://fireflymediaserver.org/
 [5]: http://www.makeuseof.com/tag/using-your-linux-computer-as-a-upnp-av-server-part-3/
 [6]: http://ushare.geexbox.org/
 [7]: http://mediatomb.cc/
 [8]: http://fuppes.ulrich-voelkel.de/
 [9]: http://www.gnu.org/software/gmediaserver/
 [10]: http://www.vuze.com/
 [11]: http://utorrent.com