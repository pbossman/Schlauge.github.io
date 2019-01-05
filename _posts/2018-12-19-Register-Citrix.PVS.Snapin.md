---
layout: post
title: Can't Add-PSSnapin Citrix.PVS.Snapin
subtitle: strong type error
published: true
---

In PowerShell, when trying to work with PVS, you need to import the PSSnapin.   When the PVS Console is installed, it also installs the Snapin, but it does not register the snap with Windows.

You can add the snapin by specifiying the fullpath to the .dll or you can register the snapin so that POwerShell can add it by name.

``` posh
Add-PSSnapin -Name "C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll"
```
This is fine, but I prefer to use shorthand

``` posh
Add-PSSnapin -Name "Citrix.PVS.SnapIn"
```
or
``` posh
Add-PSSnapin -Name "Citrix.*"
```

So, the question is... How do we register the Snapin so that we can use the short hand.

#####Register Citrix.PVS.SnapIn on PVS Server
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe 'C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll'

---
This has been my goto solution for some time.  I can register the DLL on the system and all is well.

But after I upgraded to a new version of the PVS Console(CU3 from CU1), I was no longer able to load the PVS snapin.  I got the following message
#### Snap-in failed to load
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

Well, it seems that the DLL changed enough and that the DLL makes reference to the direct version.

## Solution!
Unregister the DLL, and re-register it. The DLL paths are the same so 

### Un-Register Citrix.PVS.SnapIn
```
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe /u 'C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll'
```

### Register Citrix.PVS.SnapIn
```
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe 'C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll'
```


