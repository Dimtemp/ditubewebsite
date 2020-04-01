---
id: 83
title: Stop a remote service using PowerShell
date: 2011-11-16T10:41:21+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=83
permalink: /stop-a-remote-service-using-powershell/
categories:
  - PowerShell
---
PowerShell provides some commands to manipulate services, but that doesn&#8217;t work on remote computers! The script you can find below demonstrates that stopping a service on a remote computer using Stop-Service is not working.

First I verify that the MSDTC service is running on the local machine and a remote machine: server2. If I want to stop the service on both machines I use the Stop-Service cmdlet. But this command doesn&#8217;t work on server2 when I&#8217;m piping the output from Get-Service to Stop-Service!  🙁

There are two solutions. The first is you can use the InputObject parameter as part of Stop-Service. 

You can also use WMI to stop a remote service.  Just retreive the specific service from a remote computer by using the Get-WmiObject cmdlet and use the StopService method to stop the service.

WMI is intended to work remote so this command gives the correct result!  🙂

<pre class="brush: powershell; gutter: true">"Verifying services"
Get-Service MSDTC
Get-Service MSDTC -computer server2
"Stopping services"
Get-Service MSDTC | Stop-Service
Get-Service MSDTC -computer server2 | Stop-Service   # Stop-Service does not accept pipelined input!
"Verifying services."
Get-Service MSDTC
Get-Service MSDTC -computer server2
"Stopping remote service, alternative"
Stop-Service -InputObject (Get-Service MSDTC -ComputerName server2)
"Stopping remote service using WMI"
(Get-WmiObject win32_service -computer server2 | Where { $_.name -eq 'MSDTC' }).stopservice()</pre>

Thanks to Martin Tengvall for pointing out the -InputObject variant.

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->