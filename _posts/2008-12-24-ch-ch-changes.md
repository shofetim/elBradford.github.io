---
id: 15
title: Ch-ch-changes!
author: bradford
layout: post
guid: /?p=15
permalink: /2008/ch-ch-changes
categories:
  - Faith
  - Home Server
  - Life
tags:
  - webhostingpad.com
---
So many changes! Earlier this year I set up a family blog with a hosting service called WebHostingPad.com. I paid for 2 years, resting assured that if I didn&#8217;t like their service I could get a refund during the first 30 day &#8220;evaluation period&#8221;. Little did I know that after those 30 days my account would be moved to a slow, troubled server where it would be &#8220;down&#8221; several times a week. I use quotes, because after opening a ticket with support it miraculously came back up within an hour or so, every time. I finally quit dealing with their awful support and even worse service, opened a BBB complaint, and decided to host the site on my desktop at home. I have a quad-core Vista Ultimate machine with 4GB ram on a 10Mbit connection just sitting at home, doing nothing.<!--more-->

With the help of some awesome blog posts by the guys over at IIS.net and elsewhere I was able to set up PHP, MySQL, and WordPress on IIS 7. I will post the most helpful links below. Please note however, I was able to install WordPress 2.7, however **it did NOT WORK CORRECTLY**. Only when I downgraded to WordPress 2.5.1 was I able to access pages or postings outside of the default index.php and my site responded MUCH faster. My issues seemed to be related to url rewriting, however they were not caused by any error on my part because the issue remained with or without url rewrite enabled. After installing 2.5.1 it worked flawlessly.

  * <a href="http://learn.iis.net/page.aspx/280/wordpress-on-iis/" target="_blank">Go here to set up wordpress on IIS7. Google around on setting up PHP and MySQL on IIS7<br /> </a>
  * <a href="http://learn.iis.net/page.aspx/466/enabling-pretty-permalinks-in-wordpress/" target="_blank">To set up Pretty Permalinks &#8211; NOTE: Be sure to note the prerequisites for the URL rewrite module. I wasted a lot of time scratching my head with this one.</a>
  * <a href="http://learn.iis.net/page.aspx/246/using-fastcgi-to-host-php-applications-on-iis-70/" target="_blank">More info on setting up FastCGI with PHP on IIS 7</a>
  * <a href="http://edge.technet.com/Media/Installing-PHP-Applications-on-IIS7/" target="_blank">If you are having trouble getting PHP to work correctly, check out Ryan Dunn&#8217;s video</a>

If you have managed to get WordPress 2.7 working on IIS 7, let me know in a comment.

Before I move on with the ch-ch-changes I&#8217;ve been making, I want to give a little background on the motivation behind some of my changes. Since I was a missionary in the Philippines in 2004 I have used Yahoo! Mail, and I&#8217;ve loved their Beta mail &#8211; so much so in fact that I paid $20 a year for their premium email (I have a hard time justifying paying for ANYTHING that I can basically get for free, but I really liked Yahoo email). In addition I had a google account for all of their nifty little applications like Calendar, Apps, Blogger (for my BLOG!!), and Search. I had thousands of emails and hundreds of contacts stored in Yahoo! over the last 4 years.

During the recent passing of Proposition 8 in California amending the state constitution to prohibit homosexual marriage, I read that some big name companies in California were publicly involved. Beyond the fact that I believe that private companies should not use their resources to advance public causes. In the words of <a href="http://www.osnews.com/permalink?335006" target="_blank">rexstuff</a>,

> Whatever your views on proposition 8, it does not behoove any private company to weigh in on issues that are strictly social-political, if for no other reason than business sense&#8230;Separation of church and state? What about separation of corporation and state? It&#8217;s simply not the role of a private business company, nor should it be.

I am also morally in support of Proposition 8.  I <a href="http://newsroom.lds.org/ldsnewsroom/eng/commentary/same-sex-marriage-and-proposition-8" target="_blank">agree</a> with my <a href="http://mormon.org" target="_blank">Church</a> and believe that marriage is sacred and a re-interpretation of marriage and consequent &#8220;<a href="http://www.lds.org/library/display/0,4945,161-1-11-1,00.html" target="_blank"><span class="featurestext">disintegration of the family will bring upon individuals, communities, and nations the calamities foretold by ancient and modern prophets.</span></a>&#8221;

With such an attitude toward these current issues I was very surprised to hear that <a href="http://www.osnews.com/story/20432/Google_Apple_Openly_Support_Fight_Against_Proposition_8_" target="_blank">Apple and Google openly support the fight against Proposition 8</a> and that <a href="http://www.vator.tv/news/show/2008-10-30-yahoo-founders-against-prop-8" target="_blank">Yahoo!&#8217;s founders donated large sums of money</a> to help stop passage of Prop 8. It took me a while, obviously a few months, to come to the decision to move on from these companies and not use their services as a form of protest. I understand that I will have no impact whatsoever on their respective bottom lines, but this became a [&#8220;choose you this day whom ye will serve&#8221;][1] turning point, and so I ran with it.

To be straightforward I&#8217;ve always been disgusted with Apple&#8217;s marketing strategy and the true-to-stereotype narcissism of their users (generally speaking of course). With Google, I never really LOVED their products, but they *were *convenient. When I started my blog this summer I went with blogger because that&#8217;s what my girlfriend had, and it was good (not great) &#8211; again, convenient. The difficult service for me to give up was my Yahoo Mail. I had about 6 email addresses going into the same inbox, which was AWESOME. I had an address for all my financial institutions, one for shopping online, one from my mission, one for just junk, and one for everything else. Having disposable email addresses is really something Microsoft and Google would be wise to jump on. Plus, I had 4 years worth of email to consider. Thankfully Yahoo provides a service to download a zip file with all your emails in it.

So, I got to work. I decided to go with Microsoft Live Mail. They have all the features I liked from Google and Yahoo (except the disposable email addresses &#8211; kind of). Microsoft has Live Calendar, SkyDrive (25GB of online storage), seamless synchronization with Outlook 2007 (well, there is one seam, but you just have to install the outlook connector) which syncs all my contacts, emails, and calendars with Outlook 2007 on my desktop. Amazingly, after importing my Yahoo! emails into Outlook 2007, I was able to import my emails IN TO WINDOWS LIVE MAIL (hotmail basically) by dragging the emails from my personal folders over to the Live Mail folders. The Outlook Connector would only sync about 120 emails at a time, and I had maybe 5000, but after many many syncs, all of my Yahoo! emails are available in Windows Live Mail, in the same folders, available online or offline! Pretty amazing.

The biggest change in moving from Yahoo and Google was moving my blog from blogger to my own WordPress installation on IIS 7. After ironing out the kinks I described earlier, it was a snap importing my posts and comments from blogger into WordPress, and the plug-ins of WordPress make me glad I made the move. I think my blog is better for it, and I have a lot more freedom to do what I want with the blog.

Soon I&#8217;ll have the family blog up and running as well &#8211; and my Christmas present for my family will be complete! Albeit a little late&#8230;, but that&#8217;s why they say Merry Christmas and a Happy New Year! Because they know some people will be a little late with their gifts.

Come get some 2009! I&#8217;m ready to tackle a new year with Windows Live Mail, Calendar, Search, and WordPress at my side. Bleh, I need to drive Julie and Kimberly to the airport in an hour and a half. Should I go to sleep or play Team Fortress 2?

\*\*UPDATE\*\*

Because I hadn&#8217;t put any work into my family&#8217;s wordpress blog, I decided to see if UPGRADING would make a difference, and it did &#8211; WordPress 2.7 works great on IIS 7 after I installed 2.5.1 and then upgraded directly to 2.7. Hope that helps others who experienced the same issues.

 [1]: http://scriptures.lds.org/en/josh/24/15#15