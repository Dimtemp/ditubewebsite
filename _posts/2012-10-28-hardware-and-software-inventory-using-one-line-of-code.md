---
id: 194
title: Hardware and software inventory using one line of code (only 60 characters)
date: 2012-10-28T15:29:23+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=194
permalink: /hardware-and-software-inventory-using-one-line-of-code/
categories:
  - PowerShell
---
Recently, I was wondering how much code I would need to do basic hardware and software inventory on several remote systems. After experimenting a little bit I came up with the following line of code:

<pre class="brush: powershell; gutter: false">Get-Content pcs.txt | foreach { Write-Host $_ -f Green; $pc=$_; Get-Content wmi.txt | foreach { Get-WmiObject $_ -ComputerName $pc } }</pre>

That&#8217;s right! Just one line of code. 🙂

I&#8217;m reading the computers from a text file (pcs.txt). Then I use a foreach-clause to iterate through the list of pc&#8217;s. Write the name of the PC on the screen in green to easily identify which system we&#8217;re processing. Then I&#8217;m reading the different elements to report on from another text file (wmi.txt). For the current system in the loop I&#8217;m running a Get-WmiObject on all the items from the second text file and write that on the screen also.

I have to store the name of the current PC in a variable with the command $pc=$_ because I&#8217;m entering a nested loop. Whithin the nested loop $_ get&#8217;s filled with the current WMI-class, instead of the current PC. I can still refer to the name of the PC through the $pc variable.

Here are both textfiles. They&#8217;re just examples, feel free to supply your own items.  
wmi.txt:  
Win32_Bios  
Win32_LogicalDisk  
Win32_OperatingSystem  
Win32_Product

&nbsp;

pcs.txt:

server1  
server2

Here&#8217;s the same line of code but the using the shortest possible notation:

<pre class="brush: powershell; gutter: false">gc p|%{Write-Host $_ -f Green;$p=$_;gc w|%{gwmi $_ -Co $p}}</pre>

Sixty characters! I&#8217;m really pushing my luck here doing nasty things like abbreveating parameters (-Co instead of -Computername), using all sorts of aliases (gc for Get-Content, % for ForEach, and gwmi for Get-WmiObject) and using textfiles with a name of just one letter. But hey, for the purpose of writing the shortest line of code, it will do! 😉

Let&#8217;s get serious again. PowerShell chooses a different layout than you would expect. To compensate, just use Out-Default.

<pre class="brush: powershell; gutter: false">Get-Content pcs.txt | foreach { Write-Host $_ -f Green; $pc=$_; Get-Content wmi.txt | foreach { Get-WmiObject $_ -ComputerName $pc | Out-Default } }</pre>

With Out-Default you can&#8217;t write to a text file anymore. Instead of Out-Default, consider using Out-File to create a report.

<pre class="brush: powershell; gutter: false">Get-Content pcs.txt | foreach { "===== Computer $_ ====="; $pc=$_; Get-Content wmi.txt | foreach { Get-WmiObject $_ -ComputerName $pc } } | Out-File report.txt
 Invoke-Item .\report.txt</pre>

When you want to use computers from Active Directory the script becomes a little bit different. Be carefull when running this command in a production environment! Escpecially when it&#8217;s large!

<pre class="brush: powershell; gutter: false">Get-ADComputer -filter * | foreach { $pc=$_.dnshostname; Write-Host $pc -f Green; Get-Content wmi.txt | foreach { Get-WmiObject $_ -ComputerName $pc } }</pre>

Don&#8217;t forget to import the ActiveDirectory module if you&#8217;re still running PowerShell 2. PowerShell 3 loads this module automatically for you if it&#8217;s installed. Check wheter the AD-module is installed with this command:

<pre class="brush: powershell; gutter: false">Import-Module ServerManager; Get-WindowsFeature RSAT-AD-PowerShell</pre>

If you don&#8217;t want to use any textfiles at all then use this peace of code:

<pre class="brush: powershell; gutter: true">$wmi = "Win32_Bios", "Win32_LogicalDisk", "Win32_OperatingSystem"
Get-ADComputer -filter * | foreach { $pc=$_.dnshostname; Write-Host $pc -f Green; $wmi | foreach { Get-WmiObject $_ -ComputerName $pc } }</pre>

Using the new outputmode of Out-GridView in PowerShell 3 we can list the computers graphically, select several of them, and after the press of the OK button only the selected systems are processed.

<pre class="brush: powershell; gutter: false">Get-ADComputer -filter * | Out-GridView -OutputMode Multiple | foreach { Write-Host $_ -f Green; $pc =$_; Get-Content wmi.txt | foreach { Get-WmiObject $_ -ComputerName $pc } }</pre>

And here&#8217;s an example script file:

<pre class="brush: powershell; gutter: true"># create a collection of interesting WMI classes
$wmi = "Win32_Bios", "Win32_LogicalDisk", "Win32_OperatingSystem"</pre>

<pre class="brush: powershell; gutter: true"># get computers from AD, feel free to change the filter
Get-ADComputer -filter * |   foreach {
  # store the dnshostname for reference in the next loop
  $pc=$_.dnshostname

  Write-Host $pc -ForegroundColor Green

  # send the collection of WMI-classes into the pipe
  $wmi |       foreach {
    # run Get-WmiObject on the computer currently stored in $pc
    Get-WmiObject $_ -ComputerName $pc
  }
}</pre>

There are a lot of other interesting WMI-sources. Consider these for your WMI-selection:

Win32_Account  
Win32_Battery  
Win32_Bios  
Win32_BootConfiguration  
Win32_ComputerSystem  
Win32_DiskPartition  
Win32_Environment  
Win32_LogicalDisk  
Win32_NetworkAdapter  
Win32_OperatingSystem  
Win32_Printer  
Win32_Processor  
Win32_Product  
Win32_ScheduledJob  
Win32_Service  
Win32_Share  
Win32_TimeZone

Please send me a message if you have any questions.

Good luck!

Dimitri

&nbsp;

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->