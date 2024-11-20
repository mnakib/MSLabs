
# Deploy ADDS on Server Core

## Install the Active Directory Domain Services (ADDS) role
```
Install-WindowsFeature AD-Domain-Services –IncludeManagementTools -Verbose
```
```
Get-WindowsFeature -Name *AD*
```

## Deploy a new domain and forest

Use ADDSDeployment module cmdlets to deploy a new domain and forest, or additional domain controller

```
Get-Command -Module ADDSDeployment
```

To add a new forest and domain

```
Install-ADDSForest -DomainName mntech.com -ForestMode WinThreshold  -DomainMode WinThreshold  -DomainNetbiosName MNTECH -InstallDns:$true
```

To add a new extra DC to the Default-First-Site-Name site

```
Install-ADDSDomainController -DomainName mntech.com -InstallDns -Credential (get-credential MNTECH\Administrator) -DatabasePath "D:\ADDS\DB" -LogPath "D:\ADDS\Log" -SysvolPath "D:\ADDS\SYSVOL"
```

## Install RSAT

```
Get-WindowsFeature RSAT-AD*,RSAT-DNS*,RSAT-DHCP* | Install-WindowsFeature
```

# Deploy Exchange Server 2019

For the most updated documentation article on to install Exchange Server, use this [link](https://learn.microsoft.com/en-us/exchange/exchange-server?view=exchserver-2019)

## [Exchange Server Prereq](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/prerequisites?view=exchserver-2019#exchange-2019-prerequisites-for-preparing-active-directory)
    

Install [.NET Framework 4.8](https://download.visualstudio.microsoft.com/download/pr/014120d7-d689-4305-befd-3cb711108212/0fd66638cde16859462a6243a4629a50/ndp48-x86-x64-allos-enu.exe)

```
.\ndp48-x86-x64-allos-enu.exe /q /norestart

```

        

Install [Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679)

```
.\vcredist_x64.exe /q /norestart
```


Install RSAT

```
Install-WindowsFeature RSAT-ADDS
```


## [Windows Server Prereq for Exchange 2019](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/prerequisites?view=exchserver-2019#windows-server-2019--windows-server-2022-prerequisites-for-exchange-2019)

    


### Exchange Server 2019 Mailbox on Win2k19/Win22

Install below softwares:

[.NET Framework 4.8](https://download.visualstudio.microsoft.com/download/pr/014120d7-d689-4305-befd-3cb711108212/0fd66638cde16859462a6243a4629a50/ndp48-x86-x64-allos-enu.exe)

```
.\ndp48-x86-x64-allos-enu.exe /q /norestart
```   

[Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679)

```
.\vcredist_x64.exe /q /norestart
```

[Visual C++ Redistributable Package for Visual Studio 2013](https://support.microsoft.com/help/4032938/update-for-visual-c-2013-redistributable-package)  

```
.\vcredist_x64.exe /q /norestart
```


Install the Server Media Foundation windows feature

```
Install-WindowsFeature Server-Media-Foundation
```

Install Unified Communications Managed API 4.0

```
<DVDDriveLetter>:.\UCMARunTime\Setup.exe -q
```

Install the required Windows components

_On Windows Desktop Experience_

```
Install-WindowsFeature Server-Media-Foundation, NET-Framework-45-Core, NET-Framework-45-ASPNET, NET-WCF-HTTP-Activation45, NET-WCF-Pipe-Activation45, NET-WCF-TCP-Activation45, NET-WCF-TCP-PortSharing45, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
```

_On Windows Core_
```
Install-WindowsFeature Server-Media-Foundation, NET-Framework-45-Core, NET-Framework-45-ASPNET, NET-WCF-HTTP-Activation45, NET-WCF-Pipe-Activation45, NET-WCF-TCP-Activation45, NET-WCF-TCP-PortSharing45, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-PowerShell, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Metabase, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, RSAT-ADDS
```        
        
[Install the IIS URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)

```
.\rewrite_amd64.msi /quiet /norestart
```



### Exchange Server Edge on Win2k19/Win22

Install below softwares:

[.NET Framework 4.8](https://download.visualstudio.microsoft.com/download/pr/014120d7-d689-4305-befd-3cb711108212/0fd66638cde16859462a6243a4629a50/ndp48-x86-x64-allos-enu.exe)

```
.\ndp48-x86-x64-allos-enu.exe /q /norestart
```

[Visual C++ Redistributable Package for Visual Studio 2013](https://support.microsoft.com/help/4032938/update-for-visual-c-2013-redistributable-package)  

```
.\vcredist_x64.exe /q /norestart
```


### [Windows Client Prereq for Exchange Managment Tool](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/prerequisites?view=exchserver-2019#windows-client-prerequisites-for-the-exchange-2019-management-tools)
  
[Install the Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679)

```
.\vcredist_x64.exe /q /norestart
```    

Install the required Windows components

```
Enable-WindowsOptionalFeature -Online -FeatureName IIS-IIS6ManagementCompatibility,IIS-Metabase -All
```    

Install the Exchange Management Tools
```
<Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /Mode:Install /Roles:ManagementTools /on:"Contoso Corporation"

```
        


## [Office Online Server system requirements - Optional](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/install-office-online-server?view=exchserver-2019#office-online-server-system-requirements)

    

              

## Active Directory in Exchange Server organizations

    https://learn.microsoft.com/en-us/exchange/plan-and-deploy/active-directory/active-directory?view=exchserver-2019

### [Preparing AD for Exchange Server](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/prepare-ad-and-domains?view=exchserver-2019)

    


#### Extend the Active Directory schema

Your account needs to be a member of the Schema Admins and Enterprise Admins security groups.

```
<Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareSchema
```

#### Prepare Active Directory

Your account needs to be a member of the Enterprise Admins security group.

If you want to enable Active Directory split permissions, you must also provide the _/ActiveDirectorySplitPermissions:true_ parameter

```
<Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareAD /OrganizationName:"<Organization name>"

```

#### Prepare Active Directory Domains

If you have multiple domains in your Active Directory forest, you may prepare one single domain or all of them

```
<Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareDomain[:<DomainFQDN>]

<Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareAllDomains
```

#### Verify the installation

Chekc the log files

Using ADSI Edit, check Exchange objects on Domain, Schema and Configration partitions




## [Deploy Exchange Server](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/deploy-new-installations/deploy-new-installations?view=exchserver-2019)

    

### [Attended Installation - GUI](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/deploy-new-installations/install-mailbox-role?view=exchserver-2019)


### [Unattended Installation - Core](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/deploy-new-installations/unattended-installs?view=exchserver-2019)  

```
Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /Mode:Install /Roles:Mailbox /on:"Contoso Corporation" /InstallWindowsComponents
```
