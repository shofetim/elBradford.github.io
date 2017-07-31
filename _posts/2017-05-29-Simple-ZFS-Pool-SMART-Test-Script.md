---
published: true
title: 'Simple ZFS Pool SMART Test Script'
permalink: /2017/ZFS-SMART
author: bradford
layout: post
categories:
  - Technology
  - Scripts
tags:
  - BASH
  - ZFS
comments: true
---

I needed a simple way to run SMART tests on my ZFS pools that didn't require me to manually specify which devices to scan. The following BASH script makes it easy. It just grabs the output from zpool status and runs those against either a long test (-l) or a short test (-s).

Usage: zfs_smart.sh [-p \<POOL NAME\>] [-s\|-l]
 
 
<script src="https://gist.github.com/elBradford/5925703c69471be7f59e5a50c4289e24.js"></script>
