---
id: 401
title: Bash Script for Sitelutions.com DynDNS
author: bradford
layout: post
guid: /?p=401
permalink: /2009/bash-script-for-sitelutions-com-dyndns
categories:
  - Home Server
  - Linux
---
I use sitelutions.com for my websites and subdomains; I love using sitelutions because it&#8217;s the best free solution I found for my servers with dynamic IPs. The only problem is that there&#8217;s not very many dyndns update clients available. So, I tried my hand at scripting and this is what I&#8217;ve come up with.<!--more-->

<pre class="brush:shell">#!/bin/sh

#### SITELUTIONS DYNDNS UPDATE SCRIPT ####

USERNAME="email"
PASSWORD="password"

#Separate record ids by a comma

RECORDIDS="1234567"
TTL="600"

LOGFILE="/var/log/sitelutions.log"

### Ways to retrieve IP address ##

##(Default) use sitelutions
IP="&detectip=1"

##Retrieve from external site (HTTP)
##icanhazip.com (alternatives: ipid.shat.net/iponly, whatismyip.com, etc)
#IP=`wget -O - -q icanhazip.com`

##existing domain IP
#IP=`nslookup domain.com | grep Add | grep -v '#' | cut -f 2 -d ' '`

#Build https request
REQUEST="https://www.sitelutions.com/dnsup?user=$USERNAME&pass=$PASSWORD&id=$RECORDIDS&ip=$IP"
OUTPUT=`wget -O - -q $REQUEST`

LOG=`date +%c`" "$OUTPUT
echo $LOG &gt;&gt; $LOGFILE</pre>

Once you&#8217;ve confirmed that this works you can simply throw it in as a cronjob that runs every 30 minutes or so. I have mine run every 3 hours.

Hope this is helpful to anyone else using sitelutions! Bash scripting is pretty cool, I&#8217;m definitely going to use it more. If you have any tips on how to improve the script feel free to comment.

**Resources**:

  * <a href="https://www.sitelutions.com/help/sitelutions_dns_update.php3.txt" target="_blank">Sitelutions PHP Script</a>
  * <a href="http://bubble.gritto.net/db/query.php?id=46&ty=HOWTO" target="_blank">Resolve a hostname to IP in a bash script</a>
  * <a href="http://tips4linux.com/find-out-your-routers-external-ip-address-using-the-linux-command-line/" target="_blank">Find out your router’s external IP address using the Linux command line</a>
  * <a href="http://ubuntuforums.org/showthread.php?t=526176" target="_blank">HOWTO: Check you external IP Address from the command line</a>
  * <a href="http://snipplr.com/view/4212/append-line-to-a-file/" target="_blank">Append Line to a File</a>
  * <a href="http://www.justlinux.com/forum/showthread.php?t=140388" target="_blank">string concatenation in bash</a>

**Update 12/1/2010:** I&#8217;ve stopped updating the TTL in my script below because I&#8217;m getting an error &#8220;failure (invalid ttl)&#8221; &#8211; it appears 600 is not a good value for the TTL. You can add it back if you want, though it works without it and will simply use the existing value for TTL. More on the Sitelutions api can be found <a href="http://sitelutions.com/help/dynamic_dns_clients" target="_blank">here</a>.