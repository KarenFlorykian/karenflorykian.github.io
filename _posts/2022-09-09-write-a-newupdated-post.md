---
title: platform_temp
author: Karen
date: 2022-09-09 14:10:00 +0800
categories: [Blogging, Tutorial]
tags: [writing]
render_with_liquid: false
---
This tutorial will guide you how to write a post in the _Chirpy_ template, and it's worth reading even if you've used Jekyll before, as many features require specific variables to be set.

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
|              | ![img-description](https://karenflorykian.github.io/asssets/ubuntu_inst.png)                                                      |
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
| Debian       | The same as Ubuntu                                                                                                    |
| Centos       | In the default Centos image, there shouldn't be any timer services enabled, but better to ensure using the command: |
|              | `$ sudo systemctl list-timers`                                                                                          |
|              |                                                                                                                       |
|              | Disable timer services if required:                                                                                    |
|              | `$ sudo systemctl stop $service_name`                                                                                    |
|              | `$ sudo systemctl disable $service_name`                                                                                 |
|              | `$ sudo systemctl daemon-reload`                                                                                         |
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

The following services should be stopped/disabled: apt-daily-upgrade.service, apt-daily.service


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


3.	| Distribution | Command |
| --- | --- |
| Ubuntu | `apt install docker-compose` |
| Debian | `apt install docker-compose` |
| Centos | `sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose` <br> `sudo chmod +x /usr/local/bin/docker-compose` <br> `sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose` |
| Fedora | `sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose` <br> `sudo chmod +x /usr/local/bin/docker-compose` <br> `sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose` |


4. If hardware drive is not root drive, then mount disk to /opt directory.

4.1. Get the list of all available partitions on your system with the following command:

    ```
    fdisk -l
    ```
    
    Imagine partition name you should mount for Carrier usage is `/dev/nvme1n1`.
    
    Make a new file system:
    
    ```
    mkfs -t ext4 /dev/nvme1n1
    ```
    
    Add information about new filesystem in file system table by editing `/etc/fstab`:
    
    ```
    vi /etc/fstab
    ```
    
    Add the following line in the end of the file:
    
    ```
    /dev/nvme1n1    /opt   ext4    defaults     0        2
    ```
    
    Mount required file system to existent `/opt` directory:
    
    ```
    mount /dev/nvme1n1 /opt
    ```
    
4.2. Mount docker to `/opt` folder:

    ```
    cd /opt
    mkdir docker
    service docker stop
    mount --rbind /opt/docker /var/lib/docker
    service docker start
    ```


5.	Install Git

Check if Git is installed using the following command:
`git --version`

Otherwise install it:

| Distro | Command |
|--------|---------|
| Ubuntu | `sudo apt install git` |
| Debian | `sudo apt install git` |
| Centos | `sudo yum install git` |
| Fedora | `sudo dnf install git` |


6.	Clone carrier-io centry repository to /opt directory.
```bash
cd /opt
git clone https://github.com/carrier-io/centry.git
```
7.	Switch to next branch in century repository.
cd centry
git checkout next

8.	Get public IP of your system and set CURRENT_IP variable to defined value:

U| Distribution | Command |
|--------------|---------|
| Ubuntu | `CURRENT_IP=$(host myip.opendns.com resolver1.opendns.com | grep 'address ' | cut -d ' ' -f 4)` |
| Debian | `CURRENT_IP=$(host myip.opendns.com resolver1.opendns.com | grep 'address ' | cut -d ' ' -f 4)` |
| Centos | `yum install bind-utils`<br><br>`CURRENT_IP=$(host myip.opendns.com resolver1.opendns.com | grep 'address ' | cut -d ' ' -f 4)` |
| Fedora | `dnf install bind-utils`<br><br>`CURRENT_IP=$(host myip.opendns.com resolver1.opendns.com | grep 'address ' | cut -d ' ' -f 4)` |


9.	In .env file change DEV_IP to CURRENT_IP:

cd /opt/centry
sed -i -e "s/\$DEV_IP/$CURRENT_IP/g" .env

10.	Launch Carrier installer:
docker-compose up -d

Installation process downloads all required images from docker hub, launches images and downloads all required plugins.
Primary container is carrier-pylon. 
Once carrier-pylon container is created:
 

it is good to track installation process reading its logs using the following command:
docker logs -f carrier-pylon

 

Some connection errors might appear in the log as it tries to connect to rabbitmq while rabbit is not started yet. Due to the internal process of retry once rabbit is started errors will go away.

During installation process carrier-pylon clones all required git repos with required plugins and install dependencies.

Installation process takes ~5-10 minutes.  

The following message indicates the end of installation process:
UTC -     INFO - pylon.core.tools.server - Starting WSGI server

11.	Once installation is finished open the following URL in browser:
http://< public DNS or IP>

You should see the following page that indicates successful installation of Carrier.
 


12.	Login first time in the system using admin/admin credentials.

How to create project in Carrier
13.	Once you login you will see the following page that indicates that there are no projects created yet.
 

14.	Click to the person logo in right top corner and select Administration to switch to Administration mode and create your first project:
 

15.	Click to plus to add the project. 

Set the name of your project and click Create.
 

Your project is created. It might take ~1 minute. The process of project creation you can track in carrier-pylon logs to make sure that there are no any errors.

16.	Once project is created you can get back to Project mode
 

You will get to Project configuration page.

 

17.	Now your project is setup and you can navigate to required sections either Performance or Security to start configuring tests using left menu in dropdown list.

 

 



Troubleshooting

If something doesn’t work as expected check logs of the following containers if there are some errors:
docker logs -f carrier-keycloak
docker logs -f centry_traefik_1
docker logs -f carrier-pylon-auth


Uninstall

If root volume was used as hard drive then use the following commands to stop containers and all required artifacts.
docker-compose down
docker volume prune
docker network prune


If mounted disk was used then all directories with images should be deleted manually:
docker-compose down
rm -f /opt/docker



