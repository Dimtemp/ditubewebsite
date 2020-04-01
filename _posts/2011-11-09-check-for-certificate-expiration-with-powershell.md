---
id: 36
title: Check for certificate expiration with PowerShell (on multiple servers)
date: 2011-11-09T21:37:15+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=36
permalink: /check-for-certificate-expiration-with-powershell/
categories:
  - certificates
  - PowerShell
---
One of my clients asked me how to check for expired certificates. This simple script opens the certificate store through the PS-drive CERT: and lists all certificates that are soon to expire. You can change the threshold to any value in the first line. Here&#8217;s the script:

<pre class="brush: powershell; gutter: true">$threshold = 30   #Number of days to look for expiring certificates
$deadline = (Get-Date).AddDays($threshold)   #Set deadline date
Dir Cert:\LocalMachine\My | foreach {
If ($_.NotAfter -le $deadline) { $_ | Select Issuer, Subject, NotAfter, @{Label="Expires In (Days)";Expression={($_.NotAfter - (Get-Date)).Days}} }
}</pre>

This script can also be used for several machines at once. Just add the Invoke-Command to the Dir command and make sure PowerShell remoting has been set up by using Enable-PSRemoting on the target servers. More info by running this PowerShell command: Get-Help about_remoting.

<pre class="brush: powershell; gutter: true">$threshold = 30   #Number of days to look for expiring certificates
$deadline = (Get-Date).AddDays($threshold)   #Set deadline date
Invoke-Command -ComputerName Srv01, Srv02 { Dir Cert:\LocalMachine\My } | foreach {
If ($_.NotAfter -le $deadline) { $_ | Select Issuer, Subject, NotAfter, @{Label="Expires In (Days)";Expression={($_.NotAfter - (Get-Date)).Days}} }
}</pre>

Dimitri

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->