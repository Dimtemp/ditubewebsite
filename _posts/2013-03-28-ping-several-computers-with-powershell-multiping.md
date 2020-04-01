---
id: 207
title: 'Ping several computers with PowerShell: MultiPing'
date: 2013-03-28T14:04:41+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=207
permalink: /ping-several-computers-with-powershell-multiping/
categories:
  - Uncategorized
---
In the past I have used different tools to ping a list of computers. One of the nicest was a rather old version of WhatsUpGold. It was quick to install and simple to use.

Now we have PowerShell on almost every Windows computer I wanted to have a solution in PowerShell, so I&#8217;m not required to install any dedicated ping software anymore.

Here&#8217;s a simple script I wanted to share with you. It allows you to ping several computers at once. When a ping is over a configurable threshold output is displayed in yellow or red.

Enjoy!

Dimitri

&nbsp;

<div id="attachment_214" style="width: 687px" class="wp-caption aligncenter">
  <a href="http://www.dimensionit.tv/wp-content/uploads/2013/03/multiping1.png"><img aria-describedby="caption-attachment-214" class="size-full wp-image-214" alt="MultiPing: ping several computers" src="http://www.dimensionit.tv/wp-content/uploads/2013/03/multiping1.png" width="677" height="474" srcset="http://www.dimensionit.tv/wp-content/uploads/2013/03/multiping1.png 677w, http://www.dimensionit.tv/wp-content/uploads/2013/03/multiping1-300x210.png 300w" sizes="(max-width: 677px) 100vw, 677px" /></a>
  
  <p id="caption-attachment-214" class="wp-caption-text">
    MultiPing: ping several computers
  </p>
</div>

<pre class="brush: powershell; gutter: true">Function MultiPing {
&lt;#
.SYNOPSIS
Sends a ping to a specified host. Colors the output to indicate latency.
.DESCRIPTION
Version 1.1. Provides a simple network monitoring solution, without the need to install any software.
.EXAMPLE
MultiPing ServerX
Sends a ping to ServerX every second. Repeats forever.
.EXAMPLE
MultiPing ServerX, ServerY, 10.1.1.254, www.google.com
Sends a ping to two servers, the IP address of the default gateway and a webserver on the internet
#&gt;

param($computername="localhost", [bool]$repeat=$false, [int]$PingCritical=8, [int]$PingWarning=4)

Write-Host "Pinging $($computername.count) remote systems, repeat is $repeat. Interrupt with Ctrl-C." -Foregroundcolor green
Write-Host "Thresholds: critical=$PingCritical, warning=$PingWarning" -Foregroundcolor green
Do {
  $computername | foreach {
    $a = Test-Connection $_ -Count 1 -ErrorAction SilentlyContinue
    if (!$?) { Write-Host "$_ --- " -nonewline -fore red }   
    else {
      $msg = "$($a.Address) $($a.ResponseTime.ToString().PadRight(4))"
      if     ($a.ResponseTime -ge $PingCritical) { write-host $msg -nonewline -fore red }
      elseif ($a.ResponseTime -ge $PingWarning)  { write-host $msg -nonewline -fore yellow }
      else                                       { write-host $msg -nonewline }
    }
  }
Write-Host ""
Start-Sleep (1)
} while ($repeat)
}</pre>

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->