---
layout: post
title: Getting PVS incorrect Maintenance servers
subtitle: Find Maintenance PVS that not are not in mainteneance mode 
published: false
---

When you work with Citrix PVs, you know about the different types of images.

    Production
    Test
    Maintenance

Our implementation fo PVS uses a dedicated machine for our Maintenance machine.  We designate these machinesMM to distinguish a Maintenance machine.  Sometimes this MM machine is booted prior to setting up the new version of Maintenance.  When this happens the disk will boot Read-only.  If you dont know this

``` posh
    Get-Item
``` posh

![Crepe](\img\PVSTypes.jpg)

??? WHATS DO I what to say
