# Django Deployment With Nginx-uwsgi with docker
Here We will Depoly Our django App With Nginx and Uwsgi in a Docker Container

Nginx : NGINX is an open-source web server that offers a variety of capabilities, including:
Web serving, Reverse proxying, Caching, Load balancing, Media streaming, Email proxy, HTTP cache services. [Click-To-Know-More](https://www.nginx.com/resources/glossary/nginx/)

Uwsgi : UWSGI is a Python application framework that is an open-source software application. It is a full stack that can be used to develop and deploy web services and applications.[Click-To-Know-More](https://en.wikipedia.org/wiki/UWSGI)

Docker : Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. [Click-To-Know-More](https://docs.docker.com/get-started/overview/)

Here We Will Show Most Easyest Way to Depoly Django with this Following tech.

** One Thing To know before Going Further I Will Download Nginx Not With Docker I will Keep It In Host System To Make It Easy And Simple To Depoly More Application In Same System
But You Can Use Nginx In Docker Container And I Will Show It Very Soon.

<h1>Requirements</h1>

- Python3

- Docker
  
- Nginx
  
- Docker

# Downloading Requirments
so let's download our Requirments, before that update your system

```bash
sudo apt update
sudo apt upgrade
```
- Python3

Run This commands one by one to Download all requirments 

```bash
sudo apt install python
sudo apt install python3-pip
sudo pip install virtualenv
```

- Docker

Before Installing Docker Create a Account in [Docker-Hub](https://hub.docker.com/) (If You Already Have a Account Then There Is No Need To Create A New One )

To Install Docker Run The following Command In your VPS Server

```bash
sudo apt install docker.io docker-compose
```
After Successfully Installing Docker Genarate a New Access Token From Your Docker-Hub Profile And Loging in your System (While Genarateing New Access Token You Will Be Informed How Use The Token To  Login)

- Nginx
  
Run This commands one by one to Download all requirments and Setup Nginx

```bash
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```
Now We Have Successfully Installed Nginx Now Time To Check It's Runing Or not, TO check That run the following Command

```bash
sudo systemctl status nginx
```
** if You Nginx is not Runinng Or Working Then It Might Happening Beacuse There is any other web-Server or Something Else Which is Useing port 80

If Everything Is Fine and Nginx Is Runing Properly Then Open Your Browser From Your Local System and Paste your Server Static Ip there and You Will Able To see a Nginx Page 

<h1>FireWall Set-Up</h1>

Before Going To 


