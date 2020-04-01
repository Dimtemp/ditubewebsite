---
id: 148
title: Strange behavior with Hyper-V module from Codeplex
date: 2012-02-15T12:37:49+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=148
permalink: /strange-behavior-with-hyper-v-module-from-codeplex/
categories:
  - Hyper-V
  - PowerShell
---
When you&#8217;re using the PowerShell module for Hyper-V from James O’Neill (a.k.a. jamesone) then you&#8217;ll find out that many cmdlets don&#8217;t accept string collections as variable. Here&#8217;s an example:

<pre class="brush: powershell; gutter: false">Import-Module c:\psmodules\HyperV\HyperV.psd1
$Hosts = "HV01,HV02,HV03,HV04"
Stop-VM -VM VM03 -Server $HostsString -Force</pre>

Throws this error:  
_Get-WmiObject : The RPC server is unavailable. (Exception from HRESULT: 0x800706BA)_  
 _At c:\psmodules\HyperV\VM.ps1:73 char:26_  
 _+             Get-WmiObject <<<<  -computername $Server -NameSpace $HyperVNamespace -Query $WQL | Add-Member -MemberType ALIASPROPERTY -Name &#8220;VMElementName&#8221; -Value &#8220;ElementName&#8221; -PassThru_  
 _+ CategoryInfo          : InvalidOperation: (:) [Get-WmiObject], COMException_  
 _+ FullyQualifiedErrorId : GetWMICOMException,Microsoft.PowerShell.Commands.GetWmiObjectCommand_**  
** 

The module doesn&#8217;t interpret the $HostsString as a collection.

Here&#8217;s a possible solution:

<pre class="brush: powershell; gutter: false">Stop-VM -VM VM03 -Server $HostsString.split(",") –Force</pre>

What I&#8217;ve done is include a split-method to split the string and return it as a collection. Now the command works!

&nbsp;

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->