# Configuring Exchange Server Mailbox Databases

## Databases Management

Display existing databases

```
Get-MailboxDatabase
```

Check the databases path

```
Get-MailboxDatabase | select name,*path*
``` 

Check their mount status

```
Get-MailboxDatabase | select *mount*
```

## Move and rename the default database 


## Create a new databasse