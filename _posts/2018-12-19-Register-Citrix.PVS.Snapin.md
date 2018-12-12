---
layout: post
title: Citrix.PVS.Snapin won't load
subtitle: strong type error
---

When working with citrix.PVS.Snapin 

``` posh
PS H:\> Add-PSSnapin Citrix.PVS.Snapin
Add-PSSnapin : Cannot load Windows PowerShell snap-in Citrix.PVS.Snapin because of the following error: The Windows
PowerShell snap-in module C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll does not have
the required Windows PowerShell snap-in strong name Citrix.PVS.SnapIn, Version=7.15.0.11, Culture=neutral,
PublicKeyToken=null.
At line:1 char:1
+ Add-PSSnapin Citrix.PVS.Snapin
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidData: (Citrix.PVS.Snapin:String) [Add-PSSnapin], PSSnapInException
    + FullyQualifiedErrorId : AddPSSnapInRead,Microsoft.PowerShell.Commands.AddPSSnapinCommand
```

##Register Citrix.PVS.SnapIn on PVS Server
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe 'C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll'


##UN-Register Citrix.PVS.SnapIn on PVS Server
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe /u 'C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll'

