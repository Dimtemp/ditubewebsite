---
id: 54
title: PowerShell speaks!
date: 2011-11-13T18:16:06+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=54
permalink: /powershell-speaks/
categories:
  - PowerShell
---
The other day I stumbled upon a nice PowerShell command to use the speech API found on modern Windows Operating Systems. With this script PowerShell can talk to you! Great for situations where you want to **hear** about the progress of your script. Or maybe to **tell** you there&#8217;s an alert instead of showing the alert. Here&#8217;s the script:

&nbsp;

<pre class="brush: powershell; gutter: true">$a = New-Object -COM SAPI.SpVoice
$a.speak("I'm completely operational, and all my circuits are functioning perfectly.")
</pre>

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->