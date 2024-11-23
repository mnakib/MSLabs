# Configuring Exchange Server Mailbox Databases

## Exchange Database Management

### Display existing databases

```
Get-MailboxDatabase
```

### Check databases paths

```
Get-MailboxDatabase | select name,*path*
``` 

### Check databases mount status

```
Get-MailboxDatabase | select *mount*
```

### Rename the default database 

```
Set-Mailbox "<Mailbox Database Default Name>" -Name DB01
```

### Move a database file and logs

```
 Move-DatabasePath -Identity DB01 -EdbFilePath D:\DB01\DB01.edb -LogFolderPath E:\DB01

Get-MailboxDatabase | select name,*path*

Get-MailboxDatabase DB01 -Status | select name,*path*,*mounted*

 ```


## Create a new databasse

```
New-MailboxDatabase -Name DB03 -Server EXCH-01 -EdbFilePath D:\DB03\DB03.edb -LogFolderPath E:\DB03

Get-MailboxDatabase -Status | select name,edbfilepath,logfolderpath,mounted | ft -auto

Mount-Database DB03
```

_Restart Exchange Information Store service after a new mailbox database is created._
_Give special care to this action, as restarting EIS services will temporarily dismount all databases in the server._

```
Invoke-Command -ComputerName EXCH-01 {Restart-Service MSExchangeIS}
```