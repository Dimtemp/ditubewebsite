---
id: 76
title: Compress a folder or file using PowerShell
date: 2011-11-15T15:11:10+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=76
permalink: /compress-a-folder-or-file-using-powershell/
categories:
  - PowerShell
---
It&#8217;s possible to compress a folder or file since Windows NT. It&#8217;s a feature of the NTFS-filesystem. Normally we would do that on the properties of the folder or file in the Windows Explorer, but you can do this with PowerShell too! Just use this command:

<pre class="brush: powershell; gutter: true">Invoke-WmiMethod -Path "Win32_Directory.Name='C:\Test'" -Name compress</pre>

And to Uncompress the same folder, use this command:

<pre class="brush: powershell; gutter: true">Invoke-WmiMethod -Path "Win32_Directory.Name='C:\Test'" -Name uncompress</pre>

We&#8217;re talking about NTFS compression here, not the ZIP-feature introduced in Windows XP and later. You can notice that the folder is compressed by the blue color of the folder in Windows Explorer. Or you can open the properties of the folder to see whether the size on disk is smaller than the normal size. Do not try to compress items that are already compressed, like pictures (JPG&#8230;), music (MP3&#8230;), video (AVI, MPG&#8230;).

When you use WMI to execute operating system functions like this, you get a return value. When the return value is 0 (zero) this means the command functioned. Other return values can mean different things (read-only, no disk space, no permissions,&#8230;). In general: when we&#8217;re executing WMI-commands we&#8217;re allways checking the return value. You could implement it like this:

<pre class="brush: powershell; gutter: true">$a = Invoke-WmiMethod -Path "Win32_Directory.Name='C:\Test'" -Name compress
If ($a.returnvalue) -eq 0 { "Items successfully compressed" } else { "Something went wrong!" }</pre>

&nbsp;

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->