
# Deploy ADDS on Server Core

## Install the Active Directory Domain Services (ADDS) role
```
Install-WindowsFeature AD-Domain-Services –IncludeManagementTools -Verbose
```
```
Get-WindowsFeature -Name *AD*
```

## Use ADDSDeployment module cmdlets to deploy a new domain and forest, or additional domain controller:

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
