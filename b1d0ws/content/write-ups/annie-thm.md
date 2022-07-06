---
title: "Annie - TryHackMe"
date: 2022-07-05T13:38:50-03:00
---

https://tryhackme.com/room/annie

![bannerg-annie](/annie/banner.png)

## **User Flag**

First, I did the standard nmap scan and found that *ports 22 and 7070* are open.

Next, I did an aggressive scan on these ports to find what service is behind it and found SSH on port 22 and probably an *AnyDesk* on port 7070, recognized by the "commonName=AnyDesk Client" snippet.

![nmap-annie](/annie/nmap.png)

\
Searching for *exploits in AnyDesk* we see that there is an *RCE* vulnerability in anydesk 5.5.2. Although we do not know the version of anydesk, the attempt is valid.

![searchsploit-annie](/annie/searchsploit.png)

\
Looking inside the code we see that some changes are needed.

We have to *change the IP* to our victim IP address.

Note: the port is 50001 because this is the UDP port used by Anydesk.

We also need to *generate shellcode* with our VPN IP address using the commented msfvenom command.

![firstCode-annie](/annie/firstCode.png)

\
Generating the shellcode with my *tun0 IP* and the *port 2222*.

![shellcode-annie](/annie/shellcode.png)

\
So, the *final exploit* code is this:

![finalCode-annie](/annie/finalCode.png)

\
Now just *execute the code* with python2 with an open *listener* in the port selected and get the reverse shell.

![revShell-annie](/annie/firstRevShell.png)

\
The *user flag* is in our user home.

![userFlag-annie](/annie/user-flag.png)

## **Root Flag**

Now we need to find a way to get the root shell by privilege escalation, but first let’s try to get a *SSH session* since port 22 is open.

To do this, we access the .ssh folder and copy the private *SSH key* to our computer and try to access the machine.

![sshKey-annie](/annie/sshKey.png)

\
We set the correct permissions for the key with *chmod 600*.

![tryingSSH-annie](/annie/tryingSSH.png)

\
The key has a passphrase to login in SSH, so we need to find that password. We can do that with *ssh2john*.

First we turn the key into a hash with *ssh2john id_rsa > hashRSA*, then we use brute force with the rockyou wordlist.

Probably the password will show up in orange for you, but since I had done this CTF before, john returns me that the hash was already cracked, so I just use --show to see it.

![sshPassword-annie](/annie/sshPassword.png)

\
Now we can move on to privilege escalation.

Looking through the SUID files we find *setcap*, which is an interesting file to have a SUID bit.

![sshPassword-annie](/annie/setcapSuid.png)

Searching for how to exploit this binary to escalate to root I found this great article from Hacking Articles. I recommend reading it if you don't know the concept of capabilities.

*https://www.hackingarticles.in/linux-privilege-escalation-using-capabilities/*

The setcap command is used to *set capabilities* to other binaries. Being able to execute this as root, we can set capabilities to any file we want, so the choose was *python3*, but we can use others like perl and tar.

First, we copy python3 binary to our home, then we set the capability to it with *setcap cap_setuid_ep /home/annie/python3* command.

Now our python3 binary can run as root permission, therefore we just need to *invoke a new bash* with the UID 0 that is the root UID.

![rootFlag-annie](/annie/rootFlag.png)

See ya =).

