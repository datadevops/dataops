---
title: Linked Server on SQL Server vNext
date: 2017-05-15 12:25:46
tags:
  - vNnext
  - Linux
  - Linked Server
---

If you have started Microsoft's vNext SQL Server on Linux or Docker container and tried to set up a linked server, using SSMS GUI, you probably got this error message: <br></br>
{% img col-md-7 /images/LinkedServerError.PNG  %}

<br></br>
<br></br>The reason is that Microsoft doesn't support distributed queries on SQL Server vNext (yet), as published on Microsoft's [official site][1]:<br></br>
![](/images/LinkedServerUnsupported.PNG)


Linked server is an essential tool in SQL Server; The fact that distributed query feature is not yet supported is quite puzzling, given that [SQL Server ODBC Driver for Linux][5] is available since 2012.

But, if you are running SQL Server vNext on your dev-test Windows VirtualBox, linked server is available for you. And here you have another good reason for [starting SQL Server vNext on Windows VirtualBox][2]. 


## Linked Server on Windows VirtualBox ##

This suggested solution will only work for SQL Server vNext running on Windows VirtualBox, trying to execute it on Docker container or Linux, will output the same error as mentioned above:

**1**. Follow my [last post][2] about running SQL Server on Windows VirtualBox, download the [Vagrant box][3] (I have just updated vNext CTP 2.0), spin up the server and finally connect to it with SSMS.

**2**. Use [distributed queries stored procedures][4] to create linked server :

`EXEC master.dbo.sp_addlinkedserver` 
`@server = N'linked-server-name', `  
`@srvproduct=N'', `
`@provider=N'SQLNCLI', `
`@datasrc=N'network-name-of-sql-server or servername\instancename'`
`GO` 

**3**. Create SQL Server login on your source SQL Server, for the remote login stored procedure (@rmtuser,@rmtpassword) :

`EXEC master.dbo.sp_addlinkedsrvlogin `
`@rmtsrvname=N'linked-server-name',`
`@useself=N'False',`
`@locallogin=NULL,`
`@rmtuser=N'sql-server-user-name',`
`@rmtpassword='sql-server-user-password'`
`GO`

**4**. Your linked server is up and running.

[1]: https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-release-notes
[2]: http://sqldevops.tech/2017/04/SQL-Server-vNext-Vagrant-Box/
[3]: https://atlas.hashicorp.com/sqldevops/boxes/sql-server-vnext/versions/1.01
[4]: https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql
[5]: https://www.microsoft.com/en-us/download/details.aspx?id=28160
