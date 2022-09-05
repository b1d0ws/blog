---
title: "Dreaming - TryHackMe"
date: 2022-09-05T11:00:50-03:00
---

https://tryhackme.com/room/dreaming

![sandmanIcon](/dreaming/sandmanIcon.jpg)

This machine was made by me and my friend Julio. Our Linkedin will be in the end of this write-up in case of any doubts or suggestions you guys have =).

Note: this write-up assumes that you know how the tools being used works and understand some basic concepts like reverse shells, since this is an intermediate CTF level.

## **Recon and Helm Flag**

First, we perform a *nmap scan* to discover what ports are open.

![nmap](/dreaming/initial-nmap.png)

I perform an aggressive scan after that but nothing interesting comes up.

\
Since FTP is available, we can connect with *anonymous Login*.

![ftp](/dreaming/ftp-anonymous.png)

\
That works, so we find a zip file, but as we can see, it is protected by a *password*.

![zip-password](/dreaming/zip-password-protected.png)

\
We can crack the password with *zip2john* and extract the files.

![zip-password](/dreaming/zip-cracking.png)

\
Doing that, we have 3 text files, one of them has our *first flag* and other indicate to us that there is a website in this server and a show a possible password: *matthew333#*

![zip-cracking](/dreaming/reading-files.png)

## **First Access**

Accessing the website, it seems that there is nothing on the main page.

![website](/dreaming/website.png)

\
Doing some enumeration with *GoBuster*, we discover a hidden directory that contains a website CMS called *Pluck*.

![gobuster](/dreaming/gobuster.png)

![app](/dreaming/accessing-app.png)

![dream](/dreaming/accessing-dream.png)

\
There is a link to access *admin page* and we can access the panel with the password we found before.

![login](/dreaming/admin-login.png)

![admin](/dreaming/admin-panel.png)

\
At the bottom of the page, we discover that the CMS being used is *Pluck 4.7.13*, so we can use *searchsploit* to look for it.  

Doing that, we find a *public exploit* that can be used to create a webshell.

![searchsploit](/dreaming/searchsploit.png)

\
The usage is: *python3 exploit.py [IP] [PORT] [ADMIN PASSWORD] [PATH TO PLUCK]*

![exploit](/dreaming/using-exploit.png)

\
From the webshell we can make a *reverse shell* to get control via command line.

![webshell](/dreaming/webshell.png)

## **Lucien Flag**

As we note, our user is *www-data*, so we need to escalate privilege to other users.  

First, we upload *linpeas* to the machine using *python and wget*.

![webshell](/dreaming/linpeas-download.png)

\
Running linpeas.sh, we observe that */etc/shadow is readable* to everyone, so we can *unshadow and crack it with john*.

![linpeas-shadow](/dreaming/linpeas-shadow.png)

\
To copy these files to my machine, I simply read with cat to copy and paste into text files in my host.

![unshadow](/dreaming/cracking-lucien.png)

\
Now just login into Lucien and read her flag.

![lucien-flag](/dreaming/lucien-flag.png)

## **Death Flag**

Enumerating the machine, listing the listening ports, we discover that there is a *mysql running*.

![listing-ports](/dreaming/listing-ports.png)

We try to connect in mysql with Lucien credentials and succeed.  

There is an interesting *database called dreamers*, so we decide to take a look into it.

![mysql](/dreaming/connecting-mysql.png)

\
We see that there some data and one string called our attention that seems it is a *base64 encoded text*.  

When we decode it, we discover the *password* for death user.

![dreamers](/dreaming/mysql-password.png)

![death-flag](/dreaming/death-flag.png)

## **Morpheus Flag**

In death’s home, we see that here is a file called *restore.py*, but this file is owned by Root and we can’t execute it.

![restore](/dreaming/restore-py.png)

\
Doing more enumeration, we observe that death can run this file with python3 with *Morpheus permissions*.

![mysql](/dreaming/first-use.png)

\
Analyzing what the python code does, it is doing the backup of the dream kingdom calling a *"shutil"* library. 

Since we can run it as morpheus and it is calling a python library, we can escalate privilege with the *Python Library Hijacking* technique.

First, we copy the file of the library located in */usr/lib/python3.10/shutil.py* to */tmp/* directory, insert a *reverse shell* inside it and *call the restore.py as Morpheus*.

![copy-shutil](/dreaming/copy-shutil.png)

![shutil-py](/dreaming/shutil-py.png)

\
Finally, we get the connection with Morpheus user and find the last flag.

![morpheus-flag](/dreaming/morpheus-flag.png)

See yaa =).

[My LinkedIn](https://www.linkedin.com/in/eduardo-bido-541430193/)  
[Julio's LinkedIn](https://www.linkedin.com/in/julio-cfa/)