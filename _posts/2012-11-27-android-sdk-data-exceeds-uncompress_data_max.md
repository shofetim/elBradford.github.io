---
id: 1475
title: 'Android SDK: Data exceeds UNCOMPRESS_DATA_MAX'
author: bradford
layout: post
guid: http://theblawblog.wordpress.com/?p=1475
permalink: /2012/android-sdk-data-exceeds-uncompress_data_max
jabber_published:
  - 1354062703
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";i:0;s:6:"author";s:7:"6182409";s:7:"blog_id";s:7:"9586444";s:9:"mod_stamp";s:19:"2012-11-28 00:31:41";}'
categories:
  - Android
  - Technology
tags:
  - Android
---
I&#8217;m finishing up my first app (<a href="http://bantamstudio.co/attabase" target="_blank">attaBase</a>) and testing it on the minimum API I set: 8. That&#8217;s Android 2.2. My app grabs a 4MB CSV file and imports all the DoD locations (Military bases, etc). The problem is, the old APIs (up to 2.3.3 I think) have a limit of 1MB of uncompressed data in the assets folder. So, I had to work around this limitation.  <!--more-->

I found a tip on [Stack Overflow][1] to zip the file yourself and [use a ZipInputStream][2] to unzip it on the fly. Since I&#8217;m only ever accessing this large file once in the lifetime of my app, that&#8217;s fine. I ended up editing the Stack Overflow answers I link to above to provide examples, but here is my code that accepts a zipped file and unzips it on the fly:

```java
AssetManager am = getAssets();
InputStream is;
ZipInputStream zip;
InputStreamReader isr;
try {
    is = am.open(AttaBaseContract.IMPORT_SOURCE_ZIP);
    zip = new ZipInputStream(is);
    isr = new InputStreamReader(zip);
    zip.getNextEntry();
} catch (IOException e) {
    e.printStackTrace();
    return -1;
}

int skipline = 1;
CSVReader reader = new CSVReader(isr, CSVParser.DEFAULT_SEPARATOR, CSVParser.DEFAULT_QUOTE_CHARACTER, CSVParser.DEFAULT_ESCAPE_CHARACTER, skipline);
```

This is assuming there is only one file in the zip file, because it will only feed the first file to the CSVReader (in this case). This is a nice workaround if you&#8217;re trying to make your app compatible to an old API.

 [1]: http://stackoverflow.com/a/9423943/1486966
 [2]: http://stackoverflow.com/a/4070652/1486966