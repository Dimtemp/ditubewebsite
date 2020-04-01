---
id: 178
title: PowerShell 3 launched!
date: 2012-10-02T09:38:38+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=178
permalink: /powershell-3-launched/
categories:
  - PowerShell
---
PowerShell 3 has launched! Since september 4th you can download the final (RTM) release of PowerShell 3 from this link:

<http://www.microsoft.com/en-us/download/details.aspx?id=34595>

Some of the best new features include:

  * **Automatic module loading**, no more &#8220;import-module&#8221; required
  * Lot&#8217;s of **extra modules** like DHCP, DNS, iSCSI, NetAdapter, NetTCPIP, Printing, ScheduledTasks, SMB, and many others.
  * Few syntax changes. Get-Process | Where-Object { $_.vm -gt 150MB } can now be written as Get-Process | Where-Object vm -gt 150MB
  * Integrated Scripting Environment (ISE) includes **Intellisense**, code folding, brace matching, code snippets, block select and much more!
  * **Show-Command** let&#8217;s you use cmdlets in an interactive way.
  * **Out-GridView now has a -OutputMode parameter!** That&#8217;s really awesome! For example: Get-Process | Where vm -gt 150 MB | Kill -confirm can now be written as: Get-Process | Sort vm -Descending | Out-GridView -Outputmode multiple | Kill. This means you don&#8217;t have to experiment with the parameters (like the VM-size of 150MB) to get the correct processes. And you can confirm in the GUI! What about the Active Directory recycle bin combined with Out-GridView? I&#8217;ll write a post about that very soon.
  * **Workflows.** This is an excerpt from the documentation: &#8220;To author sequences of multi-computer management activities — that are either long-running, repeatable, frequent, parallelizable, interruptible, stoppable, or restartable — as workflows. By design, workflows can be resumed from an intentional or accidental suspension or interruption, such as a network outage, a reboot or power loss.&#8221; More info: <http://blogs.msdn.com/b/powershell/archive/tags/powershell+workflow/>
  * And one of my favorites: **PowerShell WebApplication.** This installs an IIS website where you can use PowerShell commands through a webbrowser. Internet Explorer required, you think? Google Chrome, Firefox, Safari are all supported!

I would recommend to install PowerShell 3 as soon as possible. If you&#8217;re concerned about backward compatibility then stay away from the syntax changes and new modules. But start using the ISE today. It&#8217;s awesome!

Dimitri

By the way, there&#8217;s also a new alias for select-string: sls. It should have been grep, if you ask me&#8230;  😉

&nbsp;

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->