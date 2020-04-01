---
id: 218
title: SCCM 2012 OSD troubleshooting guide
date: 2013-06-13T10:45:13+01:00
author: Dimitri Koens
layout: post
guid: http://www.dimensionit.tv/?p=218
permalink: /sccm-2012-osd-troubleshooting/
categories:
  - SCCM
tags:
  - Configuration Manager
  - OSD
  - System Center
---
Here&#8217;s a simple troubleshooting guide when running into trouble while using SCCM 2012 Operating System Deployment (OSD). Of course it&#8217;s not intended to be 100% complete. Let me know in the comments if there are any important omissions or otherwise&#8230;

  * Network connectivity: cabling, VLAN, VMware vSwitch, Hyper-V virtual network 
      * Firewall 
          * PXE requests: UDP ports 67, 68, 69, 4011
          * OS installation: 69/UDP
      * DHCP Server: lease being used
  * SCCM Administration workspace 
      * Site, Configure site components, Software Distribution 
          * Network Access account: needs read access to shares containing images and drivers
      * Distribution point properties 
          * General: certificate expiration
          * PXE: enabled, allow this DP to respond, enable unknown computer support
          * Multicast: enable multicast
          * Boundary groups
  * SCCM Software Library workspace 
      * Drivers for network and storage, distribute
      * Boot image 
          * import drivers for network and local disk access, drivers must match boot image (WinPE 2 = Vista+, unrelated to final OS)
          * Properties 
              * Customization: enable command support (F8, smsts.log)
              * data source: deploy from PXE
          * distribute
      * Operating System Installer package properties 
          * data source refers to OS media
          * Distribution Settings: Allow to be transferred via multicast
          * distribute
      * Task sequence 
          * capture image path refers to destination wim image to be created
          * account needs write access
          * deploy to specific collection or unknown computers, purpose available, boot media and PXE
  * SCCM Monitoring workspace 
      * Distribution Point Configuration Status: PXE and Multicast columns: enabled
      * System Status, Site Status: site server, distribution point, multicast service point healty
      * System Status, Component Status: SMS\_EXECUTIVE, SMS\_MULTICAST\_SERVICE\_POINT healthy
      * Reports, Task sequence* 
          * e.g. Last 1000 messages for a specific computer (Errors, warnings and information)
  * Target PC/VM 
      * Hyper-V VM: Legacy NIC (not necessary anymore since HV 2012 R2?) connected to correct virtual network
      * boot from NIC in BIOS
      * setupapi.log, netsetup.log
      * WinPE phase 
          * ipconfig: check networking, DHCP, network drivers for boot image
          * diskpart | list disk: check mass storage drivers for boot image
          * SMSTS.log: mostly found under \windows\<system32/sysWOW64/temp>, can also be under \smstslog, \_SMSTaskSequence
      * After setup: CCMSetup.log in %windir%\system32\ccmsetup

&nbsp;

<!-- AddThis Advanced Settings generic via filter on the_content -->

<!-- AddThis Share Buttons generic via filter on the_content -->