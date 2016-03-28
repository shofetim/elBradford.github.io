---
id: 1089
title: Using Lighttpd to Redirect to New URL
author: bradford
layout: post
guid: http://theblawblog.com/?p=1089
permalink: /2010/using-lighttpd-to-redirect-to-new-url
aktt_notify_twitter:
  - no
categories:
  - Home Server
  - Linux
  - Technology
---
My blog’s old domain are set to expire in November. The reason I initially registered the domain was to create a family website. My family traditionally has communicated by mass email and reply-all conversations and I thought it would help to give them something different. They rarely used it however so I decided to simply focus on my own blog. Since I have several articles that get regular hits from search engines, I worried that my articles wouldn’t be very visible. I decided to try writing up some redirect rules for my old domain in the lighttpd config file so that anyone going to my old blog links would be redirected to their new addresses.

<!--more-->

I came up with the following rules:

<pre class="brush:shell">$HTTP["host"] !~ "^(bradford).(old-website.org)$" {
    $HTTP["host"] =~ "^(.+.)?(old-website.org)$" {
    url.redirect = ("(.*)" =&gt; "http://new-website.com")
    url.redirect-code = 301
    }
}

$HTTP["host"] =~ "bradford.old-website.org" {
    url.redirect = (
    "^/(.*)$" =&gt; "http://new-website.com/$1" )
    url.redirect-code = 301
}</pre>

The first group handles any request to a subdomain of the old website EXCEPT bradford.old-website.org and redirects them to http://new-website.com with a redirect code of 301.

The second group handles any request to bradford.old-website.org and redirects them to http://new-website.com WITH the trailing address. This works wonderfully as long as you don’t change the structure of your permalinks.

Remember to enable mod_redirect under server.modules in your lightty configuration file.

Sources:

  * <a href="http://redmine.lighttpd.net/projects/lighttpd/wiki/Docs:ModRedirect" target="_blank">Lighttpd Wiki: Module: mod_redirect</a>
  * <a href="http://inebium.com/post/tips-lighttpd-url-redirect-url-rewrite" target="_blank">lighttpd: tips, url.redirect, url.rewrite, mod_proxy, etc&#8230;</a>
  * <a href="http://en.wikipedia.org/wiki/Regular_expression" target="_blank">Wikipedia: Regular Expression</a>
  * <a href="http://nixcraft.com/web-servers/13109-lighttpd-mod_redirect-example.html" target="_blank">Lighttpd mod_redirect example</a>