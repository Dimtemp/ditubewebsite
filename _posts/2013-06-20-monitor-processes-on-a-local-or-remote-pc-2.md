---
id: 227
title: Monitor processes on a local or remote PC
date: 2013-06-20T15:10:04+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=227
permalink: /monitor-processes-on-a-local-or-remote-pc-2/
categories:
  - PowerShell
tags:
  - monitoring
---
Here&#8217;s a PowerShell script that allows you to monitor processes on the local or a remote PC. It&#8217;s based on the Get-Process cmdlet. It loops every secon, compares changes in the process list and displays them nicely on the screen. It shows a green line when a new process runs and a yellow line when a process quits.

Have fun! And let me know what you think in the comments.[<img class="size-full wp-image-229 alignleft" alt="ProcessMonitor.ps1" src="http://www.dimensionit.tv/wp-content/uploads/2013/06/ProcessMonitor.jpg" width="907" height="156" srcset="http://www.dimensionit.tv/wp-content/uploads/2013/06/ProcessMonitor.jpg 907w, http://www.dimensionit.tv/wp-content/uploads/2013/06/ProcessMonitor-300x51.jpg 300w" sizes="(max-width: 907px) 100vw, 907px" />](http://www.dimensionit.tv/wp-content/uploads/2013/06/ProcessMonitor.jpg)

&nbsp;

<pre class="brush: powershell; gutter: false">Function ProcessMonitor {

&lt;#
.SYNOPSIS
Displays changes in the process list on this or a remote PC.
.DESCRIPTION
Great for monitoring logon/startup scripts, batch jobs, software installations, etc...
Version 1.2, created by Dimitri Koens
.EXAMPLE
ProcessMonitor
Compares changes in the process list every second on the local computer.
.EXAMPLE
ProcessMonitor -Interval 30
Compares changes in the process list for every 30 seconds.
.EXAMPLE
ProcessMonitor -Computername ServerB
Compares changes in the process list on server B. Requires RPC.
#&gt;

param([int]$Interval=1, [string]$Computername=&#039;.&#039;)

Write-Host "ProcessMonitor (interrupt with Ctrl-C)" -ForegroundColor Cyan

$a = Get-Process -ComputerName $Computername
Do {
  Start-Sleep $Interval
  $b = Get-Process -ComputerName $Computername
  Compare-Object $a $b -Property id -passthru | foreach {
    $msg = "{0:hh:mm:ss} {1,5} pid {2,6:N0}MB vm {3,5:N0}MB ws  {4}  {5}" -f (get-date) , $_.id, ($_.vm/1MB), ($_.ws/1MB), $_.name, $_.path
    if ($_.sideIndicator -eq "=&gt;") { Write-Host $msg -foregroundcolor green  }   # new process running
    if ($_.sideIndicator -eq "&lt;=") { Write-Host $msg -foregroundcolor yellow }   # existing process stopped
  } # foreach
  $a = $b
} while (1 -eq $true)
} # function

ProcessMonitor</pre>

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->