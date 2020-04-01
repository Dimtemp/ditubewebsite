---
id: 169
title: Microsoft SQL Server Installation Best Practices
date: 2012-05-24T11:49:54+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=169
permalink: /microsoft-sql-server-installation-best-practices/
categories:
  - SQL
---
<div>
  <div>
    Here are my personal SQL Server Installation Best Practices. Please note: they&#8217;re best practices. Maybe most of them can be hulpfull for most situations, in other situations some options must be configured otherwise. Please let me know if you have any questions or suggestions.
  </div>
  
  <ul>
    <li>
      When using 4 (or preferably more) CPU&#8217;s <strong>reserve the first core for the operating system</strong>. Configure this by opening the server (instance) properties, Processors page.
    </li>
    <li>
      <strong>Leave Boost priority off.</strong> Only use this option when really needed and you know what you&#8217;re doing. For example: when using several instances on the same server or when using another application on the same machine. Configure this by opening the server (instance) properties, Processors page.
    </li>
    <li>
      Reserve 512 MB &#8211; 2 GB of Ram for the Operating System. Configure this by opening the server (instance) properties, Memory page. <strong>Maximum server memory</strong> = Physical memory &#8211; reservation. For example: if your server has 16 GB Ram and you reserve 2 GB Ram then Maximum server memory would be 14336 (MB).
    </li>
    <li>
      When you&#8217;re not using SQL Server authentication then <strong>disable the use of SQL Logins</strong> through the server (instance) properties, Security page, Select Windows Authentication.
    </li>
    <li>
      <strong>Leave SA disabled.</strong> If it&#8217;s not disabled, log in with a Windows user as a member of the sysadmin role and disable the SA account. If you want to keep the SA account enabled, at least rename it and document the new name.
    </li>
    <li>
      Change the <strong>default backup directory path</strong>. Open Rhe registry editor and navigate to: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL.2\MSSQLServer\BackupDirectory and supply a path.
    </li>
    <li>
      Place <strong>TEMPDB</strong> on a separate volume.
    </li>
    <li>
      Create an <strong>NDF-file for the TEMPDB database for each CPU core </strong>available to SQL Server.
    </li>
    <li>
      Place your <strong>data files (MDF/NDF) on a separate volume</strong>. Configure the placement of new file  by opening the server (instance) properties, Database settings page.
    </li>
    <li>
      Place your transaction log files <strong>(LDF) on a separate volume.</strong>
    </li>
    <li>
      Leave <strong>Auto create and auto update statistics on</strong>, unless a product reconfigures otherwise (e.g. BizTalk).
    </li>
  </ul>
</div>

&nbsp;

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->