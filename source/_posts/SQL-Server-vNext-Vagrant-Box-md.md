---
title: 3 good reasons for starting SQL Server vNext.
date: 2017-04-02 11:07:02
tags:
  - vNext
  - Linux
  - Packer
  - Vagrant
  - VirtualBox
  - Ubuntu 16.04
  - Virtual Machine
---
## SQL Server vNext ##
Nov. 2016, Microsoft announced it's first SQL Server Linux version ([vNext][01]); On March 2017 CTP 1.4 was launched; An average rate of approximately one CTP update per month; Microsoft is taking things seriously!   

You probably ask yourself; Why should a SQL Server DBA, works his entire life on Windows OS,  be interested in SQL Server running on Linux?  

I'm going to give you the tools and some good reasons for running SQL Server vNext on your PC's VirtualBox, for your local Dev\Test\QA environment: 

### *1. Fast & Agile* ###
Build a "disposable" Dev\Test\QA environment; Spin up the environment (SQL Server on Linux OS) from source code in about <u>**50 sec **</u> and get a fresh new environment. If code executes not as expected, just destroy the VM and rebuild it. 

### *2. Simple* ###
Follow the instructions bellow, in just 5 min reading; Simple to maintain and configure; No previous Linux background is required; No need to change environments works on your Windows environment.

### *3. Cost effective* ###
Even though SQL Server Developer edition is free, the hosting Windows Server license, costs. You can save yourself some of those licenses, by running SQL Server on Linux.

### *4. Mobile* ###
Build a local Dev\Test\QA Lab on your Windows PC with VirtualBox; Continue working on your projects when detached from the network or have no internet access.

## Build a local Lab on your Windows PC ##
First, ensure [VirtualBox][1] and [Vagrant][2] are installed on your PC.  VirtualBox is a cross-platform virtualization application (Hypervisor) that builds virtual machines (guests) on the host.  Vagrant is easy to use tool for managing and automating the Virtual Machine(VM). 

## Vagrant Box ##
Vagrant uses a Vagrant box image to spin-up a VirtualBox VM. The image is built with the help of [Packer][3] and saved in [Vagrant box repository][4].
With the help of [Boxcutter][5] (ready made Packer templates),  I have created a SQL Server vNext 14.0.405 Vagrant box, running on Ubuntu 16.04.2 . You can view the Packer template code on [SqlDevops GitHub repo][6], and download the Vagrant Box from [SqlDevops Vagrant repo][7].

## Spin-up the Server ##
In order to spin up a VM with Vagrant, follow these 4 simple steps:
1. Create a new folder (vNext) on your host machine, and switch into it:
`c:\> mkdir vNext`
`c:\> cd vNext`

2. Start a new Vagrantfile: 
`c:\vNext> Vagrant init -m`

3. Open the Vagrantfile with your favorite text editor and copy the code below :  
`Vagrant.configure("2") do |config|` 
`config.vm.define "mssql" do |mssql|`
      `mssql.vm.box = "sqldevops/sql-server-vnext"`
      `mssql.vm.hostname = "mssql-vnext"`
      `mssql.vm.network "private_network", ip:"192.168.33.10"`
    `end`
`end`
4. Spin up the server : 
`c:\vNext> Vagrant up`

The VM's network configuration is set in the Vagrantfile; I have set it for a local network with a specific IP. More configuration settings can be found on [Vagrant Documentation][8].

First time executed, Vagrant will download the box locally (it takes a bit of a time). After the box is downloaded, it takes approx. 50 seconds to spin up a full SQL Server vNext instance with the OS from scratch.



## Connect to SQL Server vNext ##
Once the VM is up and running, it can be connected to. Open SSMS, in the ** Connect to Server **window, enter the Server's IP, as set in the Vagrant file. Use the SQL Server Authentication mode, and login  as "**sa**" with  this password  "**vnext@2017**".
If you want to name your VM's server instead of using its IP, add this row to the hosts file on your host (c:\windows\system32\drivers\etc\hosts)  , where **mssql-vnext**  is the Servers name:
`mssql-vnext 192.168.33.10`

## Open a terminal to Ubuntu Server
In case you want to open a terminal to the Linux Ubuntu OS hosting your SQL Server vNext, just execute this command:
`c:\vNext> Vagrant ssh`
For further information about managing SQL Server on Ubuntu, follow this [Link][9]

## SQL Server vNext further updates ##
Follow SQL Server vNext updates, by joining the early adoption program, on this [link][02].

[01]: https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux
[02]: http://sqlservervnexteap.azurewebsites.net/
[1]: https://www.virtualbox.org/wiki/Downloads
[2]: https://www.vagrantup.com/downloads.html
[3]: https://www.packer.io/intro/index.html
[4]: https://atlas.hashicorp.com/boxes/search?utm_source=vagrantcloud.com&vagrantcloud=1
[5]: https://github.com/boxcutter
[6]: https://github.com/sqldevops/Packer-SQL-Server-vNext
[7]: https://atlas.hashicorp.com/sqldevops/boxes/sql-server-vnext
[8]: https://www.vagrantup.com/docs/index.html
[9]: https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-management-overview

