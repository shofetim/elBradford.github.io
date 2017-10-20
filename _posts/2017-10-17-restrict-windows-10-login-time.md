---
published: true
date: '2017-10-17 15:00:00'
title: 'How to Restrict a Normal Windows 10 Account Login Hours'
permalink: /2017/restrict-win-10
author: bradford
layout: post
categories:
  - Technology
  - Scripts
tags:
  - Windows 10
comments: true
---
I'm a bit of a night owl. Lately I've found myself working late (or not working, but just staying up). When the family is asleep and the sun is down, it's difficult to tell how much time has passed. I'd like Windows to firmly remind me to go to bed when midnight rolls around. 

Windows 10 has parental controls that you may use to enforce time limits or logon hours, but the targeted account must be a designated child account. 

It is possible, however, to enforce logon hour limits *and* force a user to log off when they have crossed a time limit.

## Step 1: Set Time Limits for Your Account
Open a privileged command prompt, and use the following command: 

```powershell
net user <username> /time:<day>,<time>
```

`<day>`: This is a day or day span. The days are `Su`, `M`, `T`, `W`, `Th`, `F`, and `Sa`. A day span would be two days separated by a dash, like `Su-Sa`. 

`<time>`: This is a time span of the time the user should be allowed to log in, such as `8am-4pm`.

You may also have multiple spans of time separated by a semicolon, ala 

```
net user bradford /time:M-F,6am-8am;M-F,4pm-10pm`.
```

## Step 2: Edit Group Policy to Enforce Logon Hours
From [Superuser](https://superuser.com/a/978733/617357):

> To lock user session after logon hours expire, run the `Local Group Policy Editor` and set action to take when logon hours expire:
>
> 1. Press <kbd>Win</kbd>+<kbd>R</kbd>, then type `gpedit.msc`.
> 2. Under `User Configuration` -> `Administrative Templates` -> `Windows Components` -> `Windows Logon Options`, click on `Set Action to take when logon hours expire`.
> 3. Choose `Enabled`, then set the action to `Lock` or `Logoff`, depending on your needs.

That's it! Your account should now lock you out when you're outside your hours. Go to bed.

#### Resources
* [How-To Geek: How to Set Time Limits for a Regular Account in Windows 10](https://www.howtogeek.com/250224/how-to-set-time-limits-for-a-regular-account-in-windows-10/)
* [Superuser: Lock User Session After Logon Hours Expire](https://superuser.com/a/978733/617357)
