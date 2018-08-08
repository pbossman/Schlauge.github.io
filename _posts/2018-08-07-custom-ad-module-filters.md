---
title:  Custom ActiveDirectory Filters
---

One of my favoraite things about powershell is it's flexability.  Poiwershell can let you do so much.  The Pipeline is Awesome ( but thats another rant\post).  The focus of this post wil be about creating wrapper functions to extend the functionality of existing Cmdlets.  By defining your own function and following a few standard pratcices you can make your very own cmdlet, and make it look and operate like a compiled cmdlet

In this post we'll dicuss the Get-ADUser cmdlet from the Active Directory module.  This cmdlet is allows you to get one or more user objects.

Gets one or more Active Directory users.

   The Get-ADUser cmdlet takes a  Now you may think, "Whats wrong with Get-ADUser?"  

Based on the Help, .... You are using Get-Help, aren't you!! 
It says that Get-ADUser can be used in three ways. 
You can get a single AD User by identity   
```
Get-ADUser -Identity 
````
You can also search for a single or multiple users based on filters;
You can filter using the PowerShell Expression language
```
Get-ADUser -Filter
```
You can filter using a LDAPquery string
```
Get-ADUser -LDAPFilter
```

Now these methods are great but they are not wht way my brain works.  I want to say... Get me Peter Smith's AD account. or more likely... I want John's account...John who?... You know John M....Oh HIM!!!

So lets look at how can can solve both of these scenarios.

Lets start by defining an advanced function

``` posh {.line-numbers}
Function Get-MyADUser {}
    [CmdletBinding()]
    Param
    (
        [Parameter(Position = 0)]
        [String] $LastName,
        [Parameter()]
        [String] $FirstName,
        [Parameter()]
        [String[]] $properties,
        [Parameter()]
        [String] $Server = $env:USERDOMAIN,
        [Parameter()]
        [Switch] $Groups
    )

    Begin
    {
        If (! (Get-Command Get-ADDomain -ErrorAction SilentlyContinue) ) {
            Write-Warning "ActiveDirectory Module Not Installed...."
            Break
        }
        
        $Props = @("SamaccountName", "GivenName", "SurName")
        If ($Properties) {
            Write-Verbose "Adding additional Properties to DefaultProps..."
           Foreach ($prop in $properties) {
                $Props += $Prop
            }
        }
        If ($Groups) {
            $DefProps = $Props
            $Props += @(,"MemberOf")
        }

    }
    Process
    {

        $count = 0
        $LDAP_FirstName = ""
        $LDAP_LastName = ""

        If ($firstname) {
            $LDAP_FirstName = "(GivenName=$firstname)"
            $count++
        }

        If ($lastname) {
            $LDAP_LastName = "(SN=$lastname)"
            $count++
        }

        IF ($count -gt 1) {
            $ldapQuery = "(`&$($LDAP_FirstName)$($LDAP_LastName) )"
        } Else {
            $ldapQuery = "$($LDAP_FirstName)$($LDAP_LastName)"
        }
  
    
        $MyParms = @{ 'LDAPFilter' = "$ldapQuery" }
    
        If ($Groups) {
            $users = Get-ADUser @MyParms -Properties $Props -Server $Server -Verbose | Select -Property $Props
                $Users | % { 
                    $GroupProps = @(,"GroupName")
                    $GroupProps += $DefProps 
                    $curr = $_ 
                    Add-Member -InputObject $Curr -MemberType NoteProperty -Name "GroupName" -Value "DEFL"
                    $_.memberof | %{ 
                        $curr.GroupName = $_
                        $curr | Select $GroupProps
                    }
                    $curr = $null
                }
            

        } else {
            Get-ADUser @MyParms -Properties $Props -Server $Server -Verbose | Select $Props
        }

    }
    End
    {
    }
}

```

``` 
Get-myADUser -FirstName 

Get-MyADUser [[-LastName] <string>] [-FirstName <string>] [-properties <string[]>] [-Server <string>] [-Groups] [<CommonParameters>]

```


