---
title: "Netmon - HackTheBox"
date: 2022-07-22T14:30:50-03:00
---

https://app.hackthebox.com/machines/Netmon

![netmon-banner](/netmon/bannerNetmon.png)

## **Recon and User Flag**

First, we start with a nmap scan and discover that *ports 21, 80, 139, 445 and 5985 are open.*

Next, we perform nmap with default scripts and service detection flags on those ports.

![netmon-nmap](/netmon/nmap.png)

\
As FTP is open, we connect to it with *Anonymous login* and see that we have an entire Windows Machine to explore in it. Going directly to the Public User we already get our *user flag.*

I think this is the easiest user flag that exists in HTB.

![netmon-userFlag](/netmon/userFlag.png)

## **Root Flag**

Going to the website, we find a *PTRG Network Monitor* login page. Now we need to find the credentials of it.

The default credentials *prtgadmin:prtgadmin* doesn’t work here.

![netmon-httpSite](/netmon/httpSite.png)

\
Searching in google about PRTG Admin, we discover where it stores its data and files.

![netmon-search](/netmon/prtgSearching.png)

\
In this directory we find some interesting files, specific the *PRTG Configuration.old.bak* file.

OBS: the “ProgramData” folder sometimes is hidden in Windows.

![netmon-config](/netmon/gettingPrtgConfig.png)

\
Analyzing this file, we maybe find the *credentials* we need.

![netmon-credentials](/netmon/credentials.png)

\
Using these credentials to login in PRTG Panel or in SMB does not work.

Here we need to understand the structure of the password and change 2018 to 2019, since the credentials we found is in a old file from 2018 and there is another config file created in 2019.

So using *prtgadmin:PrTg@admin2019* works in the PRTG Panel.

![netmon-admin](/netmon/prtgAdminPanel.png)

\
Looking for PRTG vulnerability we find a *Remote Code Execution (Authenticated)* exploit that can be used.

![netmon-searchsploit](/netmon/searchsploit.png)

\
We just need to insert the *session cookies* to authenticate our exploit in command line and the exploit creates a user with administrator privilege for us.

![netmon-exploit](/netmon/exploit.png)

\
After that, we can just log in with *evil-winrm* and get the *root flag.*
![netmon-rootFlag](/netmon/rootFlag.png)

See ya =).

