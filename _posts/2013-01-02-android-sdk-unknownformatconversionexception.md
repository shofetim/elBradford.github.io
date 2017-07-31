---
id: 1328
title: 'Android SDK: UnknownFormatConversionException'
author: bradford
excerpt: Escaping percent signs ("%") in a preference summary causes a mysterious exception.
layout: post
guid: http://theblawblog.wordpress.com/?p=1328
permalink: /2013/android-sdk-unknownformatconversionexception
jabber_published:
  - 1357177594
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";i:0;s:6:"author";s:7:"6182409";s:7:"blog_id";s:7:"9586444";s:9:"mod_stamp";s:19:"2013-01-03 01:46:19";}'
categories:
  - Programming
  - Technology
tags:
  - Android
  - Android SDK
  - Percent Sign
  - PreferenceActivity
  - SettingsActivity
  - SherlockPreferenceActivity
  - UnknownFormatConversionException
---
I&#8217;ve been working on a weird exception for the past couple hours and finally figured it out. Since I couldn&#8217;t find any information about this exception I figured I&#8217;d post about it.  <!--more-->

It happens in my SettingsActivity, which extends SherlockPreferenceActivity. The cause is my string-array of values that have a percent sign in them, and when I set the preference summary that has a percent sign, the OS tries to format that percent sign. [There are a few ways to escape the percent signs][1], but none of them worked. If I used 2 %% signs, it set the summary without an exception, but it showed up as &#8220;%%&#8221; in the select list. None of the HTML encodings worked either. I even tried to HTML encode the whole string when I set the summary, but I don&#8217;t think there is a way to escape the % sign unless you escape it with another %, and that just shows up as &#8220;%%&#8221; which is unacceptable in my application. So, for now, it will just be &#8220;percent&#8221;. Lame.

Stack Trace:

> <pre>FATAL EXCEPTION: main
java.util.UnknownFormatConversionException: Conversion: 
 at java.util.Formatter$FormatSpecifierParser.unknownFormatConversionException(Formatter.java:2304)
 at java.util.Formatter$FormatSpecifierParser.advance(Formatter.java:2298)
 at java.util.Formatter$FormatSpecifierParser.parseConversionType(Formatter.java:2377)
 at java.util.Formatter$FormatSpecifierParser.parseArgumentIndexAndFlags(Formatter.java:2348)
 at java.util.Formatter$FormatSpecifierParser.parseFormatToken(Formatter.java:2281)
 at java.util.Formatter.doFormat(Formatter.java:1069)
 at java.util.Formatter.format(Formatter.java:1040)
 at java.util.Formatter.format(Formatter.java:1009)
 at java.lang.String.format(String.java:1998)
 at java.lang.String.format(String.java:1972)
 at android.preference.ListPreference.getSummary(ListPreference.java:152)</pre>

 [1]: http://stackoverflow.com/questions/4414389/android-xml-percent-symbol