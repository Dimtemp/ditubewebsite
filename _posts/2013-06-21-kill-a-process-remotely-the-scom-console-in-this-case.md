---
id: 233
title: Kill a process remotely (the SCOM Console in this case)
date: 2013-06-21T12:57:58+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=233
permalink: /kill-a-process-remotely-the-scom-console-in-this-case/
categories:
  - PowerShell
  - SCOM
---
With PowerShell you can kill a process with the Stop-Process cmdlet. While the Get-Process cmdlet let&#8217;s you get a listing of processes from a remote machine, sadly Stop-Process doesn&#8217;t allow you to connect to a remote machine.

Most possibilities in PowerShell are just .NET and WMI monikers. So as soon as PowerShell doesn&#8217;t allow me to do something, most of the time I can find a way of addressing .NET or WMI through PowerShell.

One of my students asked me whether it&#8217;s possible to kill a SCOM Console on a remote machine. Here&#8217;s how, by using WMI and PowerShell. Just replace ServerX and the name of the process with the correct names.

<pre class="brush: powershell; gutter: false"># one-liner from the PowerShell prompt
(Get-WmiObject Win32_Process -computername &#039;ServerX&#039; | where { $_.name -eq &#039;Microsoft.EnterpriseManagement.Monitoring.Console.exe&#039; }).Terminate()

# from a powershell script
param([string]$computername=&#039;.&#039;)
$a = Get-WmiObject Win32_Process -computername $computername | 
where { $_.name -eq &#039;Microsoft.EnterpriseManagement.Monitoring.Console.exe&#039; }
$a.Terminate()</pre>

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->