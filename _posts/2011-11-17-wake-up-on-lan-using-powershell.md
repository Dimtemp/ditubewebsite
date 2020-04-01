---
id: 101
title: Wake on LAN (WOL) using PowerShell
date: 2011-11-17T09:20:48+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=101
permalink: /wake-up-on-lan-using-powershell/
categories:
  - PowerShell
  - SCCM
---
There are some situations where you want to boot a remote computer that&#8217;s powered off, but don&#8217;t have the tools to do that. For example: Microsoft SCCM allows you to boot a remote computer (wake it up from power off) to install Windows Updates. If you don&#8217;t have SCCM, there are hundreds of alternative software solutions. I just don&#8217;t want to install any peace of software.

Look no further! We can power on remote computers through PowerShell! The script demonstrates how to remote boot a PC with a UDP packet we&#8217;re creating through the System.Net.Sockets.UdpClient programming interface (part of dot Net).

Just replace the MAC-address with the address of the computer you want to boot, start the script, and boot!

<pre class="brush: powershell; gutter: true">$MacAddress = [byte[]](0x00, 0x25, 0xB3, 0x0D, 0xA8, 0xF9)
$UDPclient = New-Object System.Net.Sockets.UdpClient
$UDPclient.Connect(([System.Net.IPAddress]::Broadcast),4000)
$packet = [byte[]](,0xFF * 102)
6..101 | foreach { $packet[$_] = $MacAddress[($_%6)]}
$UDPclient.Send($packet, $packet.Length)</pre>

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->