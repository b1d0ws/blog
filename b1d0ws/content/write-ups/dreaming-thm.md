---
title: "Dreaming - TryHackMe"
date: 2023-11-17T00:02:50-03:00
---

https://tryhackme.com/room/dreaming

![icon](/dreaming/icon.png)

\
This machine was made by me and my friend Julio. Our Linkedin will be in the end of this write-up in case of any doubts or suggestions you guys have =).

Note: this write-up assumes that you know how the tools being used works and understand some basic concepts like reverse shells, since this is an intermediate CTF level.

## **Foothold**

First, we perform a *nmap scan* to discover what ports are open.

![nmap](/dreaming/nmap.png)

\
Website is apache default page, so we can enumerate directories with gobuster.

![apache](/dreaming/website.png)

![gobuster](/dreaming/goBuster.png)

\
We discover a hidden directory that contains a website CMS called **Pluck**.

![app](/dreaming/app.png)

\
This seems to be a CMS that has an authenticated RCE.

![searchsploit](/dreaming/searchsploit.png)

\
Click on "admin" to login. We try some common passwords and find that "password" is the correct one.  

*Note: here we could challenge ourselves and create a script to brute force the password.*

![login](/dreaming/pluck.png)

![admin](/dreaming/login.png)

![loggedIn](/dreaming/loggedIn.png)

\
Use the exploit found with searchsploit to get a webshell.

```bash
python3 exploit.py [IP] [PORT] [ADMIN PASSWORD] [PATH TO PLUCK]
```

![usingExploit](/dreaming/usingExploit.png)

\
Upgrade to a reverse shell with mkfifo.

![gettingRevShell](/dreaming/gettingRevShell.png)

## **Lucien Flag**

As we note, our user is *www-data*, so we need to escalate privilege to other users.  

![users](/dreaming/listingUsers.png)

\
Doing some manual enumeration, we find lucien password found **/opt/test.py** and can login into SSH with it.

![lucienPassword](/dreaming/lucienPassword.png)

## **Death User**

Lucien can execute *getDreams.py* as death user without a password, but we cannot read it.

![lucienSudoers](/dreaming/lucienSudoers.png)

\
Executing it, we can’t discover many things.

![executingGetDreams](/dreaming/executingGetDreams.png)

\
Inside /opt, there is a copy of getDreams.py. Here we see a mysql connection, but the credentials are redacted.

![getDreams](/dreaming/getDreams.png)

\
Putting the code in a code editor to get the syntax highlighted, we can discover a command injection.


![findingCommandInjection](/dreaming/findingCommandInjection.png)

\
Command execution happens here:

```python
command = f"echo {dreamer} + {dream}"
```

Variables *dreamer* and *dream* come from the mysql connection, so we need to connect to it someway.  

We can find the db credentials for Lucien on her *.bash_history* file.

![dbPassword](/dreaming/dbPassword.png)

\
Enumerating the database library, we see that is here where the information from getDreams are being retrieved. 

![connectingToDB](/dreaming/connectingToDB.png)

![dbPart1](/dreaming/dbPart1.png)

![dbPart2](/dreaming/dbPart2.png)

\
Since lucien can insert into table "dreams", we can insert a payload to get us a reverse shell.  

Create a bash script file that returns a python reverse shell to be executed.

![tmpReverse](/dreaming/tmpReverse.png)

\
Insert into “dreams” table the payload above and execute getDreams.py as death.

```mysql
INSERT INTO dreams (dreamer, dream) VALUES ('aaaa; `/tmp/reverse`;', 'TEST');
```

![deathPrivEsc](/dreaming/deathPrivEsc.png)

## **Morpheus Flag**

Enumerating Morpheus home, we see that there is a restore file that backup /home/morpheus/kingdom to /kingdom_backup/kingdom.

![restoreFile](/dreaming/restoreFile.png)

\
Here you observe that *shutil library* is being used.  

Searching for this library, we see that death can write into it. Put a python reverse shell inside it and get a reverse shell as Morpheus when restore.py get executed.  


![shutilLibrary](/dreaming/shutilLibrary.png)

![shutilLibrary](/dreaming/morpheusPrivEsc.png)

*Note: here we could use pspy to confirm that restore.py is being used by a root cronjob.* 

See yaa =).

[My LinkedIn](https://www.linkedin.com/in/eduardo-bido-541430193/)  
[Julio's LinkedIn](https://www.linkedin.com/in/julio-cfa/)