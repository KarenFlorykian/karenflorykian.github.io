---
title: whatever test
author: cotes
date: 2019-08-08 14:10:00 +0800
categories: [Blogging, Tutorial]
tags: [writing]
render_with_liquid: false
---

This tutorial will guide you how to write a post in the _Chirpy_ template, and it's worth reading even if you've used Jekyll before, as many features require specific variables to be set.



# Hardware requirements:
## OS: Ubuntu, Debian, Centos, Fedora
### Carrier instance requirements:
- 4 cpu (and more)
- 16 GB RAM (and more)
- 100-500GB SSD (root drive or mounted drive)
    * Mounted drive is better option as it gives possibility to move carrier data to other instance or not lose data in case if instance is terminated accidentally.
- Static IP or public DNS

### Load generator:
- 2 cpu (and more)
- 8 GB ram (and more)

## Prerequisites
- instance launched in accordance with hardware requirements with root permissions
- the following inbound ports are open (security group for cloud):
    * http – 80
    * ssh - 22
    * tcp - 3100, 5672, 8086
- all daily updates that might trigger docker restart or termination should be disabled


## Videos

You can embed a video with the following syntax:

```liquid
{% include embed/{Platform}.html id='{ID}' %}
```
Where `Platform` is the lowercase of the platform name, and `ID` is the video ID.

The following table shows how to get the two parameters we need in a given video URL, and you can also know the currently supported video platforms.

| Video URL                                                                                          | Platform  | ID            |
|----------------------------------------------------------------------------------------------------|-----------|:--------------|
| [https://www.**youtube**.com/watch?v=**H-B46URT4mg**](https://www.youtube.com/watch?v=H-B46URT4mg) | `youtube` | `H-B46URT4mg` |
| [https://www.**twitch**.tv/videos/**1634779211**](https://www.twitch.tv/videos/1634779211)         | `twitch`  | `1634779211`  |





### Ubuntu
- Check if you have any daily services enabled using the following command:

```bash
$ sudo systemctl list-timers
```

The following services should be stopped/disabled: apt-daily-upgrade.service, apt-daily.service


The following services should be stopped/disabled: apt-daily-upgrade.service, apt-daily.service


```bash
$ sudo systemctl stop apt-daily-upgrade.timer
$ sudo systemctl disable apt-daily-upgrade.timer
$ sudo systemctl stop apt-daily.timer
$ sudo systemctl disable apt-daily.timer
$ sudo systemctl daemon-reload 
```

After disabling ensure that disabled services are not running anymore using the following command:
sudo systemctl list-timers

 

### Debian	
The same as Ubuntu

### Centos	
In default Centos image there shouldn’t any timer services enabled but better to ensure in the state of currently active timers if they are using the following command:
sudo systemctl list-timers

 

Disable timer services if required:
```bash
sudo systemctl stop $service_name
sudo systemctl disable $service_name
sudo systemctl daemon-reload 
```

###  Fedora	
In default Fedora image there shouldn’t any timer services enabled but better to ensure in the state of currently active timers if they are using the following command:
```bash
sudo systemctl list-timers
```

 
Disable timer services if required:
```bash
sudo systemctl stop $service_name
sudo systemctl disable $service_name
sudo systemctl daemon-reload 
```

## Procedure
Installation guide demonstrates installation process for Ubuntu OS (latest version).
1.	Switch to a root user
```bash
sudo su
```
2.	Install docker

Ubuntu	apt update
```bash
apt install docker.io -y
```
Debian	apt update
```bash
apt install docker.io -y
```

Centos	sudo yum install -y yum-utils
```bash
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Fedora	

```bash
$ sudo dnf -y install dnf-plugins-core

$ sudo dnf config-manager \
--add-repo \
https://download.docker.com/linux/fedora/docker-ce.repo


$ sudo dnf install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```


3.	Install docker-compose

Ubuntu	apt install docker-compose

Debian	apt install docker-compose

Centos	Check the current release by the link https://github.com/docker/compose/releases and if necessary, update it in the command below:

sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

Create a symbolic link for docker-compose in /usr/bin/docker-compose file:
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

Fedora	Check the current release by the link https://github.com/docker/compose/releases and if necessary, update it in the command below:

sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

Create a symbolic link for docker-compose in /usr/bin/docker-compose file:
   sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose


4.	If hardware drive is not root drive, then mount disk to /opt directory.
4.1. 
•	Get the list of all available partitions on your system with the following command:
fdisk -l
Imagine partition name you should mount for Carrier usage is /dev/nvme1n1.
• Make a new file system:
 mkfs -t ext4 /dev/nvme1n1
• Add information about new filesystem in file system table by editing /etc/fstab:
vi /etc/fstab
Add the following line in the end of the file: 
/dev/nvme1n1    /opt   ext4    defaults     0        2
• Mount required file system to existent /opt directory:


## Learn More

For more knowledge about Jekyll posts, visit the [Jekyll Docs: Posts](https://jekyllrb.com/docs/posts/).


> This tutorial will guide you how to instal Carrier platform.

## Hardware requirements:
system_requirements, hardware, installation

### Carrier instance requirements:
- 4 cpu (and more)
- 16 GB RAM (and more)
- 100-500GB SSD (root drive or mounted drive)
    * Mounted drive is better option as it gives possibility to move carrier data to other instance or not lose data in case if instance is terminated accidentally.
- Static IP or public DNS

### Load generator:
- 2 cpu (and more)
- 8 GB ram (and more)

## Prerequisites
- instance launched in accordance with hardware requirements with root permissions
- the following inbound ports are open (security group for cloud):
    * http – 80
    * ssh - 22
    * tcp - 3100, 5672, 8086
- all daily updates that might trigger docker restart or termination should be disabled

| Prerequisites                  |                                                                    |
|--------------------------------|--------------------------------------------------------------------|
| Instance launched with         | Hardware requirements with root permissions.                      |
| Inbound ports                  | http – 80, ssh - 22, tcp - 3100, 5672, 8086                         |
| Daily updates                  | All daily updates that might trigger docker restart or termination should be disabled. |

| Carrier instance requirements: |                    |
|--------------------------------|--------------------|
| CPU                            | 4 (and more)        |
| RAM                            | 16 GB (and more)    |
| SSD                            | 100-500GB (root drive or mounted drive) |
| IP address or DNS              | Static              |

| Load generator:                |                    |
|--------------------------------|--------------------|
| CPU                            | 2 (and more)        |
| RAM                            | 8 GB (and more)     |


| Distribution | Instructions                                                                                                          |
|--------------|-----------------------------------------------------------------------------------------------------------------------|
| Ubuntu       | Check if you have any daily services enabled using the following command:                                          |
|              | `$ sudo systemctl list-timers`                                                                                          |
|              | ![img-description](https://karenflorykian.github.io/assets/img/ubuntu_inst.png)                                                                   |
|              | The following services should be stopped/disabled: `apt-daily-upgrade.service`, `apt-daily.service`                    |
|              |                                                                                                                       |
|              | `$ sudo systemctl stop apt-daily-upgrade.timer`                                                                          |
|              | `$ sudo systemctl disable apt-daily-upgrade.timer`                                                                       |
|              | `$ sudo systemctl stop apt-daily.timer`                                                                                  |
|              | `$ sudo systemctl disable apt-daily.timer`                                                                               |
|              | `$ sudo systemctl daemon-reload`                                                                                         |
|              |                                                                                                                       |
|              | After disabling ensure that disabled services are not running anymore using the following command:                    |
|              | `$ sudo systemctl list-timers`                                                                                          |
|--------------|-----------------------------------------------------------------------------------------------------------------------|
| Debian       | The same as Ubuntu                                                                                                    |
| Centos       | In the default Centos image, there shouldn't be any timer services enabled, but better to ensure using the command: |
|              | `$ sudo systemctl list-timers`                                                                                          |
|              |                                                                                                                       |
|              | Disable timer services if required:                                                                                    |
|              | `$ sudo systemctl stop $service_name`                                                                                    |
|              | `$ sudo systemctl disable $service_name`                                                                                 |
|              | `$ sudo systemctl daemon-reload`                                                                                         |
|--------------|-----------------------------------------------------------------------------------------------------------------------|
| Fedora       | In the default Fedora image, there shouldn't be any timer services enabled, but better to ensure using the command: |
|              | `$ sudo systemctl list-timers`                                                                                          |
|              |                                                                                                                       |
|              | Disable timer services if required:                                                                                    |
|              | `$ sudo systemctl stop $service_name`                                                                                    |
|              | `$ sudo systemctl disable $service_name`                                                                                 |
|              | `$ sudo systemctl daemon-reload`                                                                                         |


### Ubuntu
- Check if you have any daily services enabled using the following command:

```bash
$ sudo systemctl list-timers
```
![img-description](https://karenflorykian.github.io/assets/img/ubuntu_inst.png) 
The following services should be stopped/disabled: apt-daily-upgrade.service, apt-daily.service

![img-description](https://karenflorykian.github.io/assets/img/ununtu1_inst.png) 
The following services should be stopped/disabled: apt-daily-upgrade.service, apt-daily.service

```bash
$ sudo systemctl stop apt-daily-upgrade.timer
$ sudo systemctl disable apt-daily-upgrade.timer
$ sudo systemctl stop apt-daily.timer
$ sudo systemctl disable apt-daily.timer
$ sudo systemctl daemon-reload 
```


## Procedure
Installation guide demonstrates installation process for Ubuntu OS (latest version).
1.	Switch to a root user
```bash
sudo su
```
2.	Install docker

| Distro  | Update Command | Installation Commands |
|---------|----------------|-----------------------|
| Ubuntu  | `apt update`    | `apt install docker.io -y` |
| Debian  | `apt update`    | `apt install docker.io -y` |
| CentOS  | `sudo yum install -y yum-utils` | `sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`<br> `sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin` |
| Fedora  | `sudo dnf -y install dnf-plugins-core` | `sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo`<br> `sudo dnf install docker-ce docker-ce-cli containerd.io docker-compose-plugin` |


3.	Install docker-compose

| Distro  | Installation Commands |
|---------|-----------------------|
| Ubuntu  | `apt install docker-compose` |
| Debian  | `apt install docker-compose` |
| CentOS  | Check the current release by the link https://github.com/docker/compose/releases and if necessary, update it in the command below:<br><br> `sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`<br><br>`sudo chmod +x /usr/local/bin/docker-compose`<br><br>Create a symbolic link for docker-compose in /usr/bin/docker-compose file:<br><br>`sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose` |
| Fedora  | Check the current release by the link https://github.com/docker/compose/releases and if necessary, update it in the command below:<br><br> `sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`<br><br>`sudo chmod +x /usr/local/bin/docker-compose`<br><br>Create a symbolic link for docker-compose in /usr/bin/docker-compose file:<br><br>`sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose` |


4.	If hardware drive is not root drive, then mount disk to /opt directory.
| Step No. | Command |
|----------|---------|
| 4.1      | Get the list of all available partitions on your system with the following command:<br>`fdisk -l`<br><br>Make a new file system:<br>`mkfs -t ext4 /dev/nvme1n1`<br><br>Add information about new filesystem in file system table by editing /etc/fstab:<br>`vi /etc/fstab`<br><br>Add the following line in the end of the file:<br>`/dev/nvme1n1    /opt   ext4    defaults     0        2`<br><br>Mount required file system to existent /opt directory:<br>`mount /dev/nvme1n1 /opt` |
| 4.2      | Mount docker to /opt folder<br>`cd /opt`<br><br>`mkdir docker`<br><br>`service docker stop`<br><br>`mount --rbind /opt/docker /var/lib/docker`<br><br>`service docker start` |