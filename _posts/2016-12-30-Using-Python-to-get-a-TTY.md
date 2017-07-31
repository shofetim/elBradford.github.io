---
published: true
title: 'Using Python to get a TTY'
permalink: /2016/Python-TTY
author: bradford
layout: post
categories:
  - Technology
  - Tutorials
tags:
  - Python
  - TTY
comments: true
---
_I grabbed this snippet from a slide posted at a CTF I attended earlier this year. It was a pretty cool little hack I decided to archive it here in case I ever need it again. Enjoy!_

```bash
$ nc -v -n -l -p 1234
listening on [any] 1234
sh: no job control in this shell
sh-3.2$ su
su must be run from a terminal
sh-3.2$ python -c 'import pty: pty.spawn("/bin/sh")'
sh-3.2$ su
su -
Password
localhost ~ #
```
