---
id: 132
title: Inconsistent BMR and system state backups with DPM
date: 2012-01-11T13:17:24+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=132
permalink: /inconsistent-bmr-and-system-state-backups-with-dpm/
categories:
  - DPM
---
With DPM 2010 you can create Bare Metal Restore (BMR) backups next to System State backups. These backups can become inconsistent more often than regular backups. This can be because there&#8217;s not enough space on the DPM server to provide a BMR backup.

It&#8217;s possible to run a test backup so you can see how large a BMR backup actually is. It also tells you what volumes are included in the backup, so you can confirm whether the correct volumes are being backed up (for instance: a D-drive with only a huge pagefile will not be backed up).

Perform the following command on the server you are protecting:

**wbadmin.exe start backup -allcritical -backuptarget:\\otherserver\share**

Replace otherserver with the name of a server that you can write a test backup to. This is not the server you are protecting. Now confirm the included volumes before starting the actual backup. Start the backup and measure the size of the backup as soon as it&#8217;s done. Make sure you have this disk space on the volume of the DPM server.

**Don&#8217;t forget to enable the Windows Server Backup feature on all Windows Server 2008 and later servers you&#8217;re protecting with DPM!** This is something the DPM Agent doesn&#8217;t do for you but it&#8217;s required for a BMR backup.

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->