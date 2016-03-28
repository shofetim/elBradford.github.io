---
id: 275
title: Windows Live Writer, lighttpd, and WordPress xmlrpc.php
author: bradford
layout: post
guid: /2009/01/26/windows-live-writer-lighttpd-and-wordpress-xmlrpcphp/
permalink: /2009/windows-live-writer-lighttpd-and-wordpress-xmlrpcphp
categories:
  - Web Design
---
I love [Windows Live Writer][1]. I use it now for all my blog editing. Recently though, after setting up my new [SheevaPlug][2] as my server and using lighttpd instead of apache as my webserver I found that I can’t use it. I got the following error trying to add my blog to my new install of WLW:

> <pre>An error occurred while attempting to connect to your blog:

Invalid Server Response - The response to the blogger.getUsersBlogs method received from the blog server was invalid:

Invalid response document returned from XmlRpc server

You must correct this error before proceeding.</pre>

<!--more-->

Doing some sleuthing, I found that this has to do with permissions on the xmlrpc.php file, which you can circumvent by adding the following code to your .htaccess file:

> <pre><span style="font-family: Courier;">&lt;Files xmlrpc.php&gt;
     SecFilterInheritance Off
&lt;/Files&gt;</span></pre>

BUT, since lighttpd doesn’t use .htaccess, I’m kind of SOL. In fact, from my internet readings, lighttpd doesn’t really have anything like mod_security.

Things were looking grim, until I stumbled across [this blog post from 2006][3]. In it, the author mentions newlines before <? in php causing problems. Lo and behold, my xmlrpc.php had some newlines before the <?. After removing them I was able to add my blog to WLW, and I’m posting from it *right now*. I hope the issues I had with uploading photos isn’t still present, but we’ll see once I make my next photo-intensive post.

 [1]: http://download.live.com/writer
 [2]: /category/sheevaplug
 [3]: http://blogs.tech-recipes.com/johnny/2006/11/01/windows-live-writer-error-bloggergetusersblogs-method-received-from-the-weblog-server-was-invalid/