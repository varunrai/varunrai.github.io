---
layout: post
title: "Installing Rancher on Ubuntu 18.04 (and newer)"
date: 2021-08-01 22:10:00 +0800
categories: rancher docker
---

In this tutorial, you will learn how to install Rancher on Ubuntu.

**Prerequisites**
* An Ubuntu system
* Access to a command-line/terminal
* A user account with sudo or root privileges
* Multiple nodes you can use for your cluster


## **Step 1: Install Docker**
1. Before downloading any new packages, always make sure to update your system:
`sudo apt update`


2. Uninstall any old Docker versions by running the command:
`sudo apt-get remove docker docker-engine docker.io`


3. Now you can install Docker with:
`sudo apt install docker.io`


4. Verify the installation was successful:
`docker --version`
![docker version](/images/b-docker-version.jpg "Docker Version")

5. Start the Docker service:
`sudo systemctl start docker`

6. Set it to run at startup:
`sudo systemctl enable docker`

7. Finally, check the status of Docker:
`sudo systemctl status docker`

The output should display the service is active (running).
![docker status](/images/b-docker-status.jpg "Docker Status")
> Note: As an alternative, you can Install Docker from Official Repository.

## **Step 2: Install Rancher**
Once you have set up Docker, use the platform to create a container where you can run the Rancher server.

1. Create a new Rancher server container using the following command:
`sudo docker run -d --restart=unless-stopped -p 8080:8080 rancher/server:stable`

> In my scenario, i had used port 8090 as 8080 was used for some other application.
> 
> Hence, my command was: 
> `sudo docker run -d --restart=unless-stopped -p 8090:8080 rancher/server:stable`

> The command above instructs Docker to run the container in detached mode and to keep it running (unless it is manually stopped). The server container is configured to listen to port 8080, but you can modify the port number according to your needs.
> Docker should pull the latest Rancher image and launch the container.

2. To access the Rancher user interface, open a web browser and type the server IP number and port in the URL bar following the syntax:
`https://[server_ip]:[port]`
![rancher status](/images/b-rancher-status.jpg "Rancher Status")

## **Step 3: Configure Rancher**
Once you have accessed the platform, Rancher instructs you to set up the Admin user (one that has full control over Rancher).

1. Open the ADMIN drop-down menu and click Access Control.
![rancher access control](/images/b-rancher-accesscontrol.jpg "Rancher Access Control")

2. Click the LOCAL button in the menu to move to the Local Authentication window.
![rancher access control - local](/images/b-rancher-accesscontrol-local.jpg "Rancher Access Control - Local")

3. Provide the required information to set up an Admin user and click Enable Local Auth to confirm.

> Rancher has many options for authentication such as Active Directory, Azure Active Directory, Github, LDAP and Local. 
> For the purpose of this demo we would use Local 