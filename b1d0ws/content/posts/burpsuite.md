---
title: "How to set up and start using Burpsuite Proxy"
date: 2022-04-13T15:20:50-03:00
---

## **Introduction**

BurpSuite is a tool with several functions used for penetration testing on web applications. It is indispensable when doing a pentest or an EHT (Ethical Hacking Test), because it allows us to do a deep analysis of the *requests and responses* made between client and server.

## **Instalation**

The software installation is trivial, just download it and follow the procedure. Available for Windows, Linux (pre-installed on Kali) and Mac, it has two versions: Community and Professional.

The community version is free to use, while the professional version is the paid version that brings some benefits like speeding up processes (brute-force in Intruder for example), additional functions and exclusive plugins.

Download Link --> https://portswigger.net/burp/communitydownload

## **Configuration**

As soon as you open the software, the Temporary project option will normally be selected in the Comunnity version. You can use the other options if you are working with an existing project or one that is to be saved if you have the Professional Edition.

![image1](/burpsuite/Image1.png)

\
Here we select the default Burp configuration because we don't have one already set up.

![image2](/burpsuite/Image2.png)

\
This is the default screen when you start BurpSuite.

![image3](/burpsuite/Image3.png)

\
In order to capture the requests made in our browser, we need to configure the browser to use a *proxy*. We can manually configure this option by going into the browser's settings, however, this would need to be done every time we use the Burp, since browsing with the proxy on all the time is not the best experience possible.

To make the process easier we use an extension called *Foxy Proxy*, which will serve as a proxy to capture our requests. 

![image4](/burpsuite/Image4.png)

\
In the upper right corner of the browser we select the extension and click options.

![image5](/burpsuite/Image5.png)

\
Click on add.

![image6](/burpsuite/Image6.png)

\
Then we apply the following settings and save.

Title - As you wish

Proxy IP - 127.0.0.1 (localhost)

Port – 8080

![image7](/burpsuite/Image7.png)

\
Once this is done, we just turn on the extension and our proxy will be active. From now on every request will be captured and pass through the Burp, so you will notice that the pages don't "walk". To stop the captures just choose the "Turn Off" option.

![image8](/burpsuite/Image8.png)

\
An extra and interesting configuration to do is to add the *PortSwigger* (Burp's owner) *Certificate* so that we can browse without interruptions warning about strange certificates.

To do this, follow the steps below.

Go to http://burpsuite/ with Burp open and the proxy enabled.

Click on *CA Certificate* in the upper right corner to download the certificate.

Then go into Firefox settings and in the *Privacy and Security* tab scroll down to the Certificates part.

![certificate](/burpsuite/Certificate.png)

\
Go to *View Certificates...* and click *Import...*

Import the newly downloaded certificate and check the Trust this CA to identify sites option and hit OK.

Now you are ready to go!

## **Proxy**

The *Proxy tab* will probably be the one you will use the most, but first let's go through the Target tab to define which websites we want to capture requests from.

#### **Defining the Scope**

In the *Site map sub-tab* click on the phrase pointed by the arrow and check the Show only in-scope items option.

![image9](/burpsuite/Image9.png)

\
In the *Scope sub-tab* we add the sites or domains we want to intercept and give a Yes to the next message.

![image10](/burpsuite/Image10.png)

\
Finally, we have to check the option indicated below located in the *Options sub-tab* of the Proxy tab.

![image11](/burpsuite/Image11.png)

Now it's much better to work, we can search other sites in the same browser without being intercepted on each request.

\
As soon as we try to access the our target site we already notice that the Proxy tab is receiving something.

![image12](/burpsuite/Image12.png)

What you are seeing is a *request*, in this case a *GET*, as we can see in line 1, that is requesting page / from the host testphp.vulnweb.com. To proceed we can click *Forward*.

Notice that we see written on the other box *"Intercept is on"*. We can click on it to disable the intercepts as well.

\
To analyze the response that the server gave us, we can right click on the request and select *Response to this request* and as soon as we click Forward we will see the response.

Basically this response brings us the site page in the body and some interesting information in the header.

![image13](/burpsuite/Image13.png)

![image14](/burpsuite/Image14.png)

In line 1 we have the communication method used (HTTP) and the response code obtained from that request (200 OK). It is important to know which are the most common response codes, so below I will leave a list of them.

On line 2 we have the information about which server is used and its respective version. This is a security flaw known as Information Disclosure.

We are telling the user what server version we are using and this can be very bad if it is outdated, because the chances of having loopholes/exploits are considerable.

On the 6th line we have another Information Disclosure, now revealing the programming language and operating system on which the application is hosted.

Under Content-Lenght is the body of the response, that is, what we will return to the users. In this case it will be an HTML page.

\
Here is a list of the most common HTTP responses:

![image15](/burpsuite/Image15.png)

## Conclusion

Now that you have Burpsuite properly configured, you can use it to perform attacks and search for vulnerabilities on web applications using the proxy to change the intercepted requests.

This was just an introduction on how to configure the tool, but you should dig deeper into it and learn how to use the other functions like Intruder and Repeater that can be very useful in the daily life of a pentester.

See ya =).

## References

https://www.geeksforgeeks.org/what-is-burp-suite/

https://portswigger.net/burp/documentation/desktop

