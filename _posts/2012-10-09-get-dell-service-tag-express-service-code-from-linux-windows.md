---
layout: post
title: Get Dell Service Tag & Express Service Code from Linux & Windows
category: dell, service tag, express service code, express service, linux, windows
---

This article is for the masses who support Dell hardware and have to reach out to Dell Support every now and then. This is an awesome trick for those times when you need to get in touch with Dell's technical support or to get drivers and other documentation from Dell. The following methods are known to be good with Windows and Linux.

# 1. Get DELL Service Tag on remote Windows System via RDP/VNC etc.

{% highlight shell %}
C:\wmic bios get serialnumber SerialNumber ABCDEF1
{% endhighlight %}

The following command can give you more information such as the make, model, and service tag.

{% highlight code %}
C:\wmic csproduct get vendor,name,identifyingnumber IdentifyingNumber Name Vendor ABCDEF1 PowerEdge 2950 Dell Inc.
{% endhighlight %}

# 2. Get DELL Service Tag on remote Windows System
If VNC or Remote Desktop isn't available, you can execute the following command from your local machine to get the service tag of the remote machine.

{% highlight code %}
C:\wmic /user:administrator /note:remote-host bios get serialnumber  
SerialNumber ABCDEF1
{% endhighlight %}

**Note:** replace remote-host with the machine name of your remote host.]

# 3. Get DELL Service Tag on remote Linux System
Login to your Linux system using SSH. Use dmidecode on Linux to get service tag as shown.

{% highlight code %}
[remote-host]# dmidecode -s system-serial-number
ABCDEF1
{% endhighlight %}

# 4. Get DELL Express Service Code from your Service Tag
Service Tag is a base-36 integer. Once you have your service tag, you can calculate your Express Service Code yourself. Express Service Code is a base-10 integer. Dell mainly uses the Service Code for support based in-call routing. When you call Dell support, it may ask you for your express service code which you can easily enter in your telephone in the dial pad, as it is a bunch of numbers. Use the following tools to find express service code from service tag and vice-versa.

1.  [Creativyst Dell-Number Widget][]
2.  [Dell PC service tag/code converter from PowerDog Industries][]

I have also tried this method on some of my other vendor equipment so far I have seen it work on the following:

1.  IBM
2.  HP Blades
3.  HP Servers
4.  Supermicro Servers

[Creativyst Dell-Number Widget]: http://www.creativyst.com/Doc/Articles/HT/Dell/DellPop.htm
[Dell PC service tag/code converter from PowerDog Industries]: http://www.powerdog.com/dellconv.cgi
