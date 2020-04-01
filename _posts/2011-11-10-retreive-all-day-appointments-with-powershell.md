---
id: 41
title: Retreive all-day appointments in Outlook with PowerShell
date: 2011-11-10T08:51:42+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=41
permalink: /retreive-all-day-appointments-with-powershell/
categories:
  - Microsoft Office
  - Outlook
  - PowerShell
---
Not too long ago I migrated my e-mail, calendar and contacts to the cloud. The migration was very smooth except for one thing. For some unknown reason a reminder was added to all my all-day appointments. I use these appointments for birthdays and that sort of events. I never use a reminder on my all-day appointments but after the migration all these appointments were set with a reminder of 15 minutes before the appointment. A daily appointment starts at 0:00 and lasts until 24:00. So that means my phone urged me to wake up at 23:45 so I didn&#8217;t forget my sisters birthday!

The next morning I realised what went wrong and that I could expect a lot of nightly wakeup calls if I didn&#8217;t remove the reminders. I have a lot of daily events and was wondering if PowerShell was able to build a list of all-day events with a reminder set.

The following script uses the Outlook COM-object to retreive all your events. It selects all-day events that have a reminder set. **Please note:** in most cases Outlook will show a pop-up or maybe a pop-under where you&#8217;re asked for access to Outlook from PowerShell.

&nbsp;

<pre class="brush: powershell; gutter: true"># Create the outlook application object, and connect to the calendar folder
$olApp = New-Object -COM Outlook.Application
$namespace = $olApp.GetNamespace("MAPI")
$fldCalendar = $namespace.GetDefaultFolder(9)
"Retreiving calendar items"
$items = $fldCalendar.Items
$items | Where { $_.reminderset -eq $true -and $_.alldayevent -eq $true -and $_.isrecurring -eq $true } |
Sort Start | Format-Table Start, Subject -auto | out-file .\agenda-reminders.txt
</pre>

&nbsp;

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->