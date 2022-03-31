---
title: "Anthem - TryHackMe"
date: 2022-03-31T13:38:50-03:00
---

https://tryhackme.com/room/anthem

![gif](/anthem/anthem.gif)

## **Website Analysis**

First of all, we run nmap and check which ports are open. As you can see, I ran a version scan with default scripts on the ports I found which were 80 (HTTP) and 3389 (RDP) to see if I could find anything interesting, but nothing turned up.

![nmap-anthem](/anthem/nmap.png)

\
Next I checked the robots.txt file and found a possible password.

![robots.txt](/anthem/robots.txt.png)

\
I also checked the disallowed entries and found a login page in /umbraco/, so we need to find our user since we already have the password!

![login](/anthem/login.png)

\
Navigating through the site, we can enumerate two users in the articles. In the first one we can get the Jane Doe user and also the domain of the website: anthem.com. We can also see that there is an email pattern, the first letters of the user name + @anthem.com.

![domain](/anthem/domain.png)

\
In the second article we can enumerate the user James Orchard Halliwell, but there is something more interesting in the text.

James said he made a poem to the admin. If we google this poem, we can find the administrator’s name and consequently guess his email with the email pattern.

![article](/anthem/article.png)

## **Spot the Flags**

It was possible to find 3 flags just by checking the source code in different pages.

![flag1](/anthem/flag1.png)

![flag2](/anthem/flag2.png)

![flag4](/anthem/flag4.png)

\
Navigating through the site you can find the other flag in the Jane Doe author page. I found it when I was enumerating users.

![flag3](/anthem/flag3.png)

## **Final Stage**

Using the email and password we found for the administrator, we can successfully login in the website. So, I also tried to login via RDP with the same credentials (without the domain) and was able to get the first access to the machine. The user's flag appears right on the desktop.

![flag-user](/anthem/flag-user.png)

\
After that, I was looking for the machine's administrator password and got stuck at that point. I took a look at the hint and enabled the option to view hidden items in Windows.

By doing this, I was able to find a txt that would probably have the administrator's password, but I couldn't open it.

I didn't have permission to read the file, so I simply changed the permissions and was able to get the admin password.

![permissions](/anthem/change-permissions.png)

\
Now we just have to login in the machine via RDP and get the root flag that is also in the desktop.

![root-flag](/anthem/root-flag.png)

See ya =).

