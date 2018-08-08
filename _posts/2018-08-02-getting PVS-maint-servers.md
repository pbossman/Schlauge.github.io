---
layout: post
title: Getting PVS Maintenance Servers
subtitle: Find Maintenace PVS that not
---

If you work with Citrix PVs, you will know about the different types of images.

    Production
    Test
    Maintenace

Our implementation fo PVS uses a dedicated machine for our Maintenance manchine.  We designate these machinesMM to distiguish a Maintenance machine.  Sometimes this MM machine is booted prior to setting up the new version of Maintenance.  When this happens the disk will boot Read-only.  If you dont knowtic this

``` posh
    Get-Item
``` posh

![Crepe](\img\PVSTypes.jpg)