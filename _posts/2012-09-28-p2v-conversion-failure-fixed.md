---
layout: post
title: P2V Conversion Failure Fixed
date: 2012-09-28
categories: p2v, vmware, windows 2000, windows server p2v
---

We’ve been trying to convert a few Windows 2000 SP4 server to a VMWare machine for a while and running into the same issue.  I would always get a MODEEXCEPTIONNOT_HANDLED BSOD right after the login screen would appear.  I tried the normal fixes listed on the VMWare website including the right SP4 rollup 1 v2 and checking the versions of scsiport.sys.  Fortunately, I stumbled across this wonderful article: <http://www.networkworld.com/news/2005/041105-windows-crash.html?page=1>.

I set the virtual machine to a kernel mode dump and let it BSOD as recommended by the article.  I then copied to memory.dmp file to my Windows 7 workstation where I had installed the debugging tools referenced in the article.  I quickly identified usbsp.sys as the offending driver.  I renamed it on the virtual and rebooted.  Result?  Perfect!  I will definitely make sure I use this in the future to get a handle on blue screens when they pop up. Here is the relevant data from the process with the info highlighted in red.   How cool is that?

The important bits to know is this section here: **SYMBOL_NAME:  usbsp+af5 FOLLOWUP_NAME:  MachineOwner MODULE_NAME: usbspIMAGE_NAME:  usbsp.sys**

{% highlight shell %}
EXCEPTION_PARAMETER1:  00000000EXCEPTION_PARAMETER2:  00000000 ERROR_CODE: (NTSTATUS) 0 - STATUS_WAIT_0 BUGCHECK_STR:  0x1E_0
DEFAULT_BUCKET_ID:  DRIVER_FAULT PROCESS_NAME:  System LAST_CONTROL_TRANSFER:  from f2695af5 to 8042be0b STACK_TEXT: f245fc78
f2695af5 0000001e 00000001 804b1cd8 nt!KeBugCheck+0xf WARNING: Stack unwind information not available. Following frames may be
wrong. f245fc90 804b1d5e 828696f0 82614000 f219fd08 usbsp+0xaf5 f245fd58 804b1f9f 0000008c 82614000 f219fd08
nt!IopLoadDriver+0x672 f245fd78 80417b47 f219fd08 00000000 00000000 nt!IopLoadUnloadDriver+0x3f f245fda8 80457838 f219fd08
00000000 00000000 nt!ExpWorkerThread+0xaf f245fddc 8046c8e6 80417a98 00000001 00000000 nt!PspSystemThreadStartup+0x54 00000000
00000000 00000000 00000000 00000000 nt!KiThreadStartup+0x16 STACK_COMMAND:  kb FOLLOWUP_IP: usbsp+af5 f2695af5 8d45f4         
lea     eax,[ebp-0Ch] SYMBOL_STACK_INDEX:  1 SYMBOL_NAME:  usbsp+af5 FOLLOWUP_NAME:  MachineOwner MODULE_NAME:
usbspIMAGE_NAME:  usbsp.sys DEBUG_FLR_IMAGE_TIMESTAMP:  3cc859dd FAILURE_BUCKET_ID:  0x1E_0_usbsp+af5 BUCKET_ID:
0x1E_0_usbsp+af5 Followup: MachineOwner
{% endhighlight %}
