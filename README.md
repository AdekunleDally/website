EXPLORING LXC/LXD CONTAINERS:How to host a website on a LXD container

This project describes how you would host the website on a LXD container


## Documentation

[Documentation](https://linktodocumentation)

Serving a website from a LXD container involves a series of steps. In this example, the LXD container would be based off an ubuntu image. Nginx server would also be installed inside the LXD container in order to access it using a public i.p. address via a web browser. The steps below describe how to host and serve a website from a LXD container.
1.	Install LXD on the Ubuntu terminal using the command below                                                      sudo snap install lxd  –channel=3.0/stable
2.	Check the LXD version :    lxc –version
3.	Add the user called cloud_user to the lxd group using the command 
sudo usermod cloud_user -aG lxd
4.	Initialise LXD in order for the LXD container to be placed in a storage pool and for it to have network access. Initialise LXD container using the command below:
lxd init
5.	Launch a LXC/LXD container off an ubuntu-based image using the lxc launch command below:
lxc launch images:ubuntu/20.04 myWeb-container
6.	Pull down the website folder from GitHub into the Ubuntu terminal.
7.	Access the container using the lxc exec command to update the container and also install nginx using the command below
lxc exec myWeb-container -- /bin/bash
8.	Update and upgrade the ubuntu container using the following commands respectively
apt update 
apt upgrade
9.	Push the myWeb folder on the Ubuntu terminal home directory to www folder which is located in the ubuntu-based container called myWeb-container  using the lxc file push command
lxc file push myWeb/  myWeb-container/var/www/  --create-dirs  -r
10.	Access the container and install nginx while inside the container using the apt install nginx command
apt install nginx  
   
11.	 Nginx has a config file for a default server block. You need to edit this file to determine what would be served by nginx. This file is located in /etc/nginx/sites-available.  Edit this file by using the command 
Vi   /etc/nginx/sites-available/default
12.	  Add the following commands inside the ‘default’ file
root /var/www/myWeb/;
index  index.html;
13.	Install Uncomplicated firewall(ufw) and configure it. Ufw is used to configure firewall rules on the lxd container which in turn helps to control the incoming and outgoing traffic to and from the container. Install ufw using the commands below
sudo apt install ufw
sudo systemctl start ufw
sudo systemctl enable ufw
sudo ufw allow 80/tcp
sudo ufw enable
sudo ufw status
systemctl status nginx

14.	Find out the private ip address of the container using the ‘lxc list’ command
lxc list

15.	Expose the web service running inside the container to the outside world. This allows incoming HTTP requests to be directed to the ubuntu container(myWeb-container), where Nginx web server can handle the requests. Run the command below to do this


lxc config device add myWeb-container myDevice proxy listen=tcp:0.0.0.0:80 connect=tcp:10.129.147.192:80




## Authors

- [@AdekunleDally](https://www.github.com/AdekunleDally)




