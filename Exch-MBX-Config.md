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

 ```




## Create a new databasse