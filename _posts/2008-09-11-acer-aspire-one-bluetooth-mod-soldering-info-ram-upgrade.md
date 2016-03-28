---
id: 1512
title: 'Acer Aspire One Bluetooth Mod (Soldering Info) &#038; Ram Upgrade'
author: bradford
layout: post
guid: /2008/09/11/acer-aspire-one-bluetooth-mod-soldering-info-ram-upgrade/
permalink: /2008/acer-aspire-one-bluetooth-mod-soldering-info-ram-upgrade
blogger_blog:
  - bradfordslaptap.blogspot.com
blogger_author:
  - Bradfordnoreply@blogger.com
blogger_permalink:
  - /2008/09/acer-aspire-one-bluetooth-mod-soldering.html
categories:
  - Technology
tags:
  - Acer Aspire One
  - mod
---
I have been in the market for a netbook ever since Asus announced their ~$200 laptop EEEPC way back, which started a major trend. And now that Asus has released a dozen or so models of the EEEPC and other manufacturers have released their own copycats, I gave the netbook market another look.<!--more-->

A [recent article][1] on Engadget caught my eye, revealing an announced price drop on the Aspire One to $350 for the Windows XP version with the 120GB HDD and 1GB RAM. I was interested, and when I saw a Best Buy coupon for $100 off any laptop and an advertisement showing the Aspire One in stock for the following week, I decided to jump.  
[<img class="alignleft size-thumbnail wp-image-1396" title="OLYMPUS DIGITAL CAMERA" src="https://theblawblog.files.wordpress.com/2008/09/p1010030.jpg?w=150" alt="" width="150" height="112" />][2]After the initial setup I cleaned off as much of the crap that is pre installed as I could and changed a few settings. I use Vista on my desktop and I was interested in the possibility of throwing Vista on this little guy. After reading a few posts at [aspireoneuser.com][3] about other people installing Vista on their Aspire One and the subsequent battery performance gains (Vista is much better at power management from what I&#8217;ve read). I decided to jump. I mucked around for a while trying to get a bootable usb stick with a vista install image on it but eventually had to give up and use a USB CD drive I found here at work.

Install was fast, and the performance with a full Vista Ultimate install was impressive. Aero worked perfectly after installing the latest Intel display driver. I used the SDHC reader slot found on the left side, normally used for SSD augmentation, as a permanent readyboost drive with a 4GB SDHC card.

After install I followed [this ][4]guide to tweaking Vista written by TweakHound. I partitioned the HDD into two drives and made some adjustments with the document folders and with virtual memory. All the details are found in the guide which is quite extensive and well written.

At this point I installed all my apps, including Office 2007 ultimate, RocketDock, Blackboard Backpack, Firefox, etc and had a GREAT little netbook.

Having read [this ][5]article on Engadget, I had on my list of to-dos a bluetooth install and memory upgrade as demonstrated by tnkgrl on [her blog.][6]

I watched her videos and I felt good about opening up the Aspire One. I took some decent notes, as the station I was going to use for my work didn&#8217;t have a computer available. I also noted some of her pictures and printed them out for reference. I will be marking them up and using them below, however the originals are all credited to tnkgrl. To see them more closely click on them. To download very high resolution versions of the photos visit [tnkgrls flicker album][7] for this project. Most of the images are VERY HIGH RESOLUTION, though some are slightly blurred.

<span style="font-weight: bold;">Take Apart:</span>

I simply followed [this video][6] by tnkgrl posted on her blog for the take apart and [this video][8] for reassembly.

I did accidentally damage the front clip which is on the same piece as the trackpad so be very gentle when you&#8217;re removing the case (which is underneath the keyboard). The clip still held when I reassembled, but I don&#8217;t think it will take much more damage. Also, be sure to keep the screws in separate piles and remember where the short screws come from and where the long screws come from. I had to guess, which I hate doing for projects like this.

Also, when removing the clip holding the keyboard and trackpad ribbon cables, the clip came completely off the mount. DON&#8217;T LOSE these. I don&#8217;t know whether they were supposed to come off or not, however I didn&#8217;t apply excessive pressure. It looks like they&#8217;re just cheap or built like that out of necessity. In any case, you can put them back on, just make sure they are put back on right-side-up.

To access the RAM you have to remove the entire motherboard &#8211; and in my version with the 2.5&#8243; HDD I had to also remove the daughter board. It&#8217;s straight-forward and I had no trouble with this part. Watch the video linked above if you want to see what this looks like.

<span style="font-weight: bold;">RAM:</span>

Installing ram was incredibly simple after the trials endured in unpacking the motherboard. If only I knew what was to await me in the bluetooth dept&#8230;. Anyway, I popped out the 512MB 5300 SODIMM piece and slapped in a Netlist 1GB 5300 DDR2 piece, which worked beautifully by the way. When I rebooted and windows took a long time loading I nearly fainted, but I think that was Windows adjusting to the new hardware. Windows doesn&#8217;t like hardware changes.

<span style="font-weight: bold;">Bluetooth:</span>

This was the hardest part, and this is why I wrote this post. tnkgrl had excellent videos on how to unpack the thing, but not much on how to solder the thing together, which is where I spent the majority of the 3 1/2 hours performing this mod. I&#8217;m not a skilled solder-ite by any means, but I do have experience. Luckily I was in my University&#8217;s EE lab with some great equipment and a helpful lab tech to help me out.

Here are some tips:

  1. Find the smallest, pointiest iron tip you can find. Having a bulky soldering-iron tip can make it hard to solder those tiny wires on the tiny contacts.
  2. Use fine wire &#8211; tnkgrl recomments 30ga, I used some 22ga and 26ga (it was scrap wire) and had trouble with the big bulky wire (22ga and 26ga is big and bulky?! it is when the application is so @#$# small). Use 30ga. Also, when you strip it, make sure to leave only a tiny amount of wire exposed, because you don&#8217;t want the exposed wire shorting anything out.
  3. When soldering on the mini-pci solder dots, tin the tip of the wire first, then simply place the tinned wire tip on the solder dot. Put the tip of the solder iron (which should not be too hot) at the joint for only as long as it takes to melt the solder that is on the wire. If you need to, lightly poke the joint until there is a bond. The problem here is that the terminals are SO CLOSE TOGETHER that it is easy to get solder overflow onto the next terminal, which leads me into my next tip.
  4. Use an ohm-meter to measure the resistance between adjoining terminals. If you have very low resistance that means there is a short and you need to clean up the terminal using a braid or something. If you have high resistance that means there is no connection. I had access to a great ohm-meter that had an alarm for low resistance, so I didn&#8217;t have to move my head up from the terminals of the meter.
  5. This is very small stuff, and while I have great near vision, it was too much for me. Have access to a bright lamp and to a magnifier. Preferably one of the magnifiers you can place in your eye. I had one that I had to get really close for proper focus and I burned my nose on the soldering iron so BE CAREFUL! 😀
  6. Because each wire going to the mini-pci terminals had some exposed wire that could potentially short if moved, I used hot-glue to insulate them from each other. It also helped keep them in place. You may also use a silicone glue for the same effect.

Enough of the tips for now, here&#8217;s an overview of what I did.

<div style="text-align: center;">
  <p style="text-align: center;">
    <a href="https://theblawblog.files.wordpress.com/2008/09/2762558852_687221d6ee.jpg"><img class="aligncenter size-full wp-image-1397" title="2762558852_687221d6ee" src="https://theblawblog.files.wordpress.com/2008/09/2762558852_687221d6ee.jpg" alt="" width="375" height="500" /></a>
  </p>
  
  <p>
    <span style="font-size: 78%;"><em>Original photo by <a style="text-align: center;" href="http://www.flickr.com/photos/tnkgrl/">tnkgrl</a></em></span>
  </p>
</div>

I purchased [this ][9]bluetooth dongle for $30 &#8211; $10 mail in rebate. I took off the case and soldered 4 wires on the usb terminal connectors, just like in the image below.

<div style="text-align: center;">
  <p style="text-align: center;">
    <a href="https://theblawblog.files.wordpress.com/2008/09/2761713249_82c3ec7970_b.jpg"><img class="aligncenter size-full wp-image-1398" title="2761713249_82c3ec7970_b" src="https://theblawblog.files.wordpress.com/2008/09/2761713249_82c3ec7970_b.jpg" alt="" width="640" height="480" /></a>
  </p>
  
  <p>
    <span style="font-size: 78%;"><em>Original photo by <a style="text-align: center;" href="http://www.flickr.com/photos/tnkgrl/">tnkgrl</a></em></span>
  </p>
</div>

<span style="font-size: 78%;"><em><br /> </em></span>It is important to note that when soldering stuff like this, it is important to NOT heat up the contact as high as you would in other soldering applications. High temperatures can definitely damage sensitive components. Even though you don&#8217;t get the same bond strength it is strong enough for our application. So, follow tip number 3 and you&#8217;ll get adequate solders and they will be much cleaner.

After I made sure the connection was good with a multi-meter, I put some shrink tube around the adapter (except the antennae) and made it look nice and professional.

Next, I soldered the +5V and the ground. In the picture below it is not perfectly clear where you solder the wires. The GND wire is soldered onto the right end of the beige colored chip. It&#8217;s big, and there&#8217;s not much else around it so it is an easy solder. The +5 is a little tougher though. Solder the +5 wire to the bottom right terminal of the black chip in the circle. It is difficult to stay away from the little tan chip at the bottom of the pink circle so be sure to use an ohm-meter if you&#8217;re not sure about whether they&#8217;re isolated.

<div style="text-align: center;">
  <p style="text-align: center;">
    <a href="https://theblawblog.files.wordpress.com/2008/09/2761713081_002b5874fa_b.jpg"><img class="aligncenter size-full wp-image-1399" title="2761713081_002b5874fa_b" src="https://theblawblog.files.wordpress.com/2008/09/2761713081_002b5874fa_b.jpg" alt="" width="640" height="480" /></a>
  </p>
  
  <p>
    <span style="font-size: 78%;"><em>Original photo by <a style="text-align: center;" href="http://www.flickr.com/photos/tnkgrl/">tnkgrl</a></em></span>
  </p>
</div>

Now for the mother of solders. Like I said, I&#8217;m not terribly experienced and this was really tough for me. In the picture below, you can see that we solder onto part of the mini-pci slot. Note, the USB- goes 9 terminals up from the bottom and USB+ is 8 from the bottom. It is very difficult to solder this, so follow my tip number 3 and if you suspect there is a short, TEST IT. You don&#8217;t want to have to open this up later to fix it, or worse yet, fry your little lappy.

<div style="text-align: center;">
  <p style="text-align: center;">
    <a href="https://theblawblog.files.wordpress.com/2008/09/2761712947_3448c59a44_b.jpg"><img class="aligncenter size-full wp-image-1401" title="2761712947_3448c59a44_b" src="https://theblawblog.files.wordpress.com/2008/09/2761712947_3448c59a44_b.jpg" alt="" width="640" height="853" /></a>
  </p>
  
  <p>
    <span style="font-size: 78%;"><em>Original photo by <a style="text-align: center;" href="http://www.flickr.com/photos/tnkgrl/">tnkgrl</a></em></span>
  </p>
</div>

Just remember, have a LOT OF LIGHT, a strong magnification source, fine solder, fine soldering iron tip, steady hands, and patience. I didn&#8217;t have enough light, strong enough magnification, a fine iron tip, steady enough hands, nor enough patience, so it took longer than it should have. In the end it looked very similar to this:

<div style="text-align: center;">
  <p style="text-align: center;">
    <a href="https://theblawblog.files.wordpress.com/2008/09/2761713397_e684b19b72_b.jpg"><img class="aligncenter size-full wp-image-1402" title="2761713397_e684b19b72_b" src="https://theblawblog.files.wordpress.com/2008/09/2761713397_e684b19b72_b.jpg" alt="" width="640" height="480" /></a>
  </p>
  
  <p>
    <span style="font-size: 78%;"><em>Original photo by <a style="text-align: center;" href="http://www.flickr.com/photos/tnkgrl/">tnkgrl</a></em></span>
  </p>
</div>

<span style="font-weight: bold;">Reassembly:</span>

Putting it together wasn&#8217;t that hard after having watched [tngrl&#8217;s reassembly vid][8]. I taped down the wire to the board using small strips of standard electrical tape (not ideal but all I had access to at the time), and placed the bluetooth adapter in the same spot as tnkgrl.

I put all the screws back in place and had everything but the keyboard back on. I would recommend plugging the keyboard in and just try to boot up without reassembling everything (of course, make sure all the cables are in place first) to make sure it boots up and that you can see the new ram and even boot into windows to see if the bluetooth dongle is found. If all is well, proceed with reassembly.

So, after 3 1/2 hours and that many heart attacks my first &#8220;hardcore&#8221; mod is done! I am looking forward to modding this with a gps adapter and 3G when T-Mobile gets 3G in my market. If you have any questions or corrections or even pointers just leave a comment. Credit to tnkgrl for her awesome blog and for fearlessly leading the way to voiding warranties.

Edit 12 September 2008: I started to notice strange behavior of the keyboard and trackpad (the $ symbol wouldn&#8217;t work, or the enter key wouldn&#8217;t work). Turns out that I managed to &#8220;break&#8221; the clips the ribbon cables for the keyboard and trackpad hook into. I was expecting them to be the kind of connectors that slide out, and I pushed so hard on them that the black connector piece came off.

After the funny behavior I mentioned, I took it apart and found that the black connector pieces flip up to allow the ribbon cable to be removed or reattached. I was able to reconnect the black pieces to the connector correcly, allowing for a solid connection.

Because of this, I am quite good at removing the keyboard. My tip is to pop all the clips that will stay popped in first (sometimes the clips will stay popped in, locked, others won&#8217;t so they have to be pressed in constantly) and then push in any others (there are only 3 so hopefully you have at least one that stays in because you have only 2 hands). If you push it in with a credit card as tnkgrl suggests, you can get the credit card behind the metal keyboard base, and if that&#8217;s the case, you can pry it up and pop it out. What happened to me was that usually it would pop up a bit, but not enough to get the credit card behind it, and if I would try to pry the credit card behind the metal base, it would just push the keyboard back ito place. So, I would pry the keyboard up slightly, very gently, under one of the keys (be very careful when doing this) and I used another card (a nice and thin Best Buy Reward Card) to get under the metal base for a more secure hold. Hope this information helps someone.

Also, I have added credits to the photos.

 [1]: http://www.engadget.com/2008/08/22/acer-remembers-netbooks-were-supposed-to-be-cheap-drops-price-o/
 [2]: https://theblawblog.files.wordpress.com/2008/09/p1010030.jpg
 [3]: http://www.aspireoneuser.com/
 [4]: http://tweakhound.com/vista/tweakguide/index.htm
 [5]: http://www.engadget.com/2008/08/14/acer-aspire-one-not-immune-to-tnkgrls-modding-ways-stuffed-wit/
 [6]: http://tnkgrl.wordpress.com/2008/08/14/modding-the-acer-aspire-one-bluetooth/
 [7]: http://www.flickr.com/photos/tnkgrl/sets/72157606718788110/
 [8]: http://qik.com/video/183950
 [9]: http://www.circuitcity.com/ssm/Kensington-Bluetooth-USB-Micro-Adapter-33902US/sem/rpsm/oid/203939/rpem/ccd/productDetail.do