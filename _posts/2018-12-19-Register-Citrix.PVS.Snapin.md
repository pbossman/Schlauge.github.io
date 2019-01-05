---
layout: post
title: Can't Add-PSSnapin Citrix.PVS.Snapin
subtitle: strong type error
published: true
---

In PowerShell, when trying to work with PVS, you need to import the PSSnapin.   When the PVS Console is installed, it also installs the Snapin, but it does not register the snap with Windows.

You can add the snapin by specifying the full path to the .dll or you can register the snapin so that PowerShell can add it by name.

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

#### Register Citrix.PVS.SnapIn with full path

``` posh
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe 'C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll'
```

---

Registering the snapin has been my go-to solution for some time.  If I register the DLL on the system, I don't have to worry about getting the path correct and I can easily add the snapin from the console.

But I recently discovered a issue after I upgrade my PVS environment from 7.15 CU1 to 7.15 CU3.  After I installed the PVS Console, I was no longer able to load the PVS snapin.  I got the following message

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

Well, it seems that the registration includes version information, so when the versions change, the load failed.  

## Solution!

Unregister the DLL (old version), and re-register the DLL (new version).

#### Un-Register Citrix.PVS.SnapIn

``` posh
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe /u 'C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll'
```

#### Register Citrix.PVS.SnapIn

``` posh
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe 'C:\Program Files\Citrix\Provisioning Services Console\Citrix.PVS.SnapIn.dll'
```
