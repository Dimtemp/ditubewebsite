---
id: 125
title: Compare your baseline using Best Practices Analyzer and PowerShell
date: 2011-12-06T12:43:38+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=125
permalink: /compare-your-baseline-using-best-practices-analyzer-and-powershell/
categories:
  - Best Practices
  - PowerShell
---
With the built-in Best Practices Analyzer we can run several tests and implement any out comings. The BPA is incorporated in the Windows Operating System since Windows Server 2008. With PowerShell we can run a BPA-scan, store it as a baseline and compare it with our current situation. Here is a PowerShell script to establish the baseline:

<pre class="brush: powershell; gutter: true">$BpaModel = "Microsoft/Windows/WebServer"
$BaselineFile = "baseline.xml"
Import-Module BestPractices
Invoke-BpaModel $BpaModel
Get-BpaResult $BpaModel | Export-CliXML $BaselineFile</pre>

And using this script we can compare the current situation with our baseline:

<pre class="brush: powershell; gutter: true">$BpaModel = "Microsoft/Windows/WebServer"
$BaselineFile = "baseline.xml"
Import-Module BestPractices
Invoke-BpaModel $BpaModel
$Bpa = Get-BpaResult $BpaModel
$BpaBaseline = Import-CliXML $BaselineFile
Compare-Object $BpaBaseline $Bpa -property Severity, Title, Resolution |
     Where { $_.SideIndicator -eq "=&gt;" }</pre>

You can replace the first line of both script with the name of your model, for example: Microsoft/Windows/DNSServer. You can query for all the installed BPA models using Get-BpaModel. Popular Best Practices that are included with Windows are: Active Directory, DNS and IIS.

I can recommend you schedule the second script every week or so, and e-mail the results as soon as a change is detected.

Dimitri

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->