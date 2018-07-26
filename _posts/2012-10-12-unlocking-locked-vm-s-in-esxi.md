---
layout: post
title: Unlocking Locked VM's in ESXi
category: virtualization, vmware, esxi, virtual machine
---

So one of your virtual machines either won't lock or got locked in ESXi and you need to fix it quickly? Well VMware's KB can be pretty useful when it's direct and to the point. However I found it difficult to figure out after a bit of digging around in my ESXi system. So I thought I would clarify this for everyone here. Often times you'll probably see one of these errors:

*   A virtual machine cannot power on.
*   Powering on a virtual machine fails.
*   Unable to power on a virtual machine.
*   Adding an existing disk (VMDK) to a virtual machine that is already powered on fails with the error:

{% highlight code %}
Failed to add disk scsi0:1. Failed to power on scsi0:1
{% endhighlight %}

When powering on the virtual machine you may see one of these errors:

*   Unable to open Swap File
*   Unable to access a file since it is locked
*   Unable to access virtual machine configuration

In the ***/var/log/vmkernel*** log file, you see entries similar to:

{% highlight code %}
WARNING: World: VM xxxx: xxx: Failed to open swap file <path>: Lock was not free  

WARNING: World: VM xxxx: xxx: Failed to initialize swap file <path>
{% endhighlight %}

When opening a console to the virtual machine, you receive this error:

{% highlight code %}
Error connecting to <path><virtual machine>, vmx because the VMX is not started
{% endhighlight %}

*   Powering on the virtual machine results in the power on task remaining at 95% indefinitely.
*   Cannot power on the virtual machine after deploying it from a template.
*   The virtual machine reports conflicting power sates between VMWare vCenter Server and the ESXi host console.
*   Attempting to view or open the .vmx file using the *cat* or *vi* command reports the error:

{% highlight code %}
cat: can't open '[name of vm].vmx': Invalid argument
{% endhighlight %}

Any one of the above error statements that is output by your ESXi server there is an easy fix for it: The virtual machine files are commonly locked for run time use (i.e. when the host is running the machine) are usually any of the files in the directory. The easiest way to identify the locked file is to attempt to power on the VM and you will see an error message that will give you the full path to the VM's directory plus the locked file. So the work around is as follows:

1.  Login to the ESXi host via SSH.
2.  Confirm that the virtual machine is registered on the server and obtain the full path to the VM. Run the following command:

{% highlight code %}
# vmware-cmd -l
{% endhighlight %}

The output returns a list of VM's registered to the host server. Should look similar to this:

{% highlight code %}
[<datastore>] <VMDIR>/<VMNAME>.vmx
{% endhighlight %}

You the need to move the VM's working directory:

{% highlight code %}
cd /vmfs/volumes/<datastore>/<VMDIR>
{% endhighlight %}

You can use any text editor to view the vmware.log file, near the end it will have the most verbose output related to your error. The command is really easy to fix the whole unlock/lock issue for the whole directory:

{% highlight code %}
touch *
{% endhighlight %}

You should now be able to start up your VM without any issues.
