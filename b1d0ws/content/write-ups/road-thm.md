---
title: "Road - TryHackMe"
date: 2023-01-09T15:30:50-03:00
---

https://tryhackme.com/room/road

![road-banner](/road/roadBanner.png)

## **Recon and User Flag**

First, we do a nmap scan to discover that port 22 and 80 are open. 

![nmap](/road/nmap.png)

\
Visiting the website, there is a "Merchant Central" button that led us to an admin login page.

![website](/road/website.png)

![loginPage](/road/loginPage.png)

\
We can register our user.

![registering](/road/registering.png)

\
After the login, we see just a normal dashboard.

![dashboard](/road/dashboard.png)

\
In the "ResetUser" option, we see that we can alter our user password.  
But what if we can alter the request to another user?

![resetPassword](/road/resetPassword.png)

![resetPasswordRequest](/road/resetTestPasswordRequest.png)

\
Now we need to discover another username to change it password.  
Looking at the profile configurations, there is a change profile image function that only admins can access, and there is a possible username for the admin: admin@sky.thm.

![profile](/road/profile.png)

![changeProfileImage](/road/changeProfileImage.png)

\
Let’s modify the change password request to this username.

![resetingAdmin](/road/resetingAdmin.png)

\
Apparently, it worked!

![resetingSucessful](/road/resetSucessful.png)

\
Now we just login with the new password and get permission to update profile images, so we can upload a php reverse shell here.

![uploadReverseShell](/road/uploadReverseShell.png)

\
But now we need to find where this file is stored.  
Looking at the source code of this profile page, we find a comment reveling where the profile images are stored.

![profileImages](/road/profileImages.png)

\
Direct access is not permitted in this directory, so we need to access the file in http://10.10.28.242/v2/profileimages/php-reverse-shell.php.

![profileImagesForbidden](/road/profileImagesForbidden.png)

\
Now we get the reverse shell and cat the user.txt file.

![ncAndUserFlag](/road/ncAndUserFlag.png)

## **Webdeveloper User Access**

Doing some port enumeration, we see that there is an open port number 27017. This is the port of mongo database.

![openPorts](/road/openPorts.png)

\
Accessing the mongo, in the database "backup"  and collection "user", we found the webdeveloper password.

![mongo](/road/mongoDB.png)

\
With this, we have SSH access.

![ssh](/road/sshLogin.png)

## **Root Flag**

Doing further enumeration, we see that we can use /usr/bin/sky_backup_utility with LD_PRELOAD set environment permission with the root user.
With that we can execute the command with our own libraries and escalate our privileges.

![sudo](/road/sudo.png)

\
First, we create a C file to compile our library.

![shell.c](/road/shell.c.png)

\
Then we compile it, execute the /usr/bin/sky_backup_utility with our library and get the root flag.

![shell.c](/road/rootFlag.png)