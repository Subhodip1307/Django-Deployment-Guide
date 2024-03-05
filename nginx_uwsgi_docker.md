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
Now We Have Successfully Installed Nginx Now Time To Check whether It's Running Or not, To check That Run the following Command

```bash
sudo systemctl status nginx
```
** if Your Nginx is not running or Working Then It Might Happening because there is any other web-Server or Something Else That is Useing port 80

If Everything Is Fine and Nginx Is running properly Then Open Your Browser From Your Local System and Paste your Server Static Ip there and You Will Able To see a Nginx Page 

<h1>FireWall Set-Up</h1>

Before Going To Further We Need To Set-Up FireWall To allow Nginx to Run The Following Commands

```bash
sudo ufw enable
sudo ufw allow 'Nginx Full'
```
- Nginx Full: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)

Now Let's Check The Status

```bash
sudo ufw status verbose
```
After Running We Will Able To see 'Nginx Full' (If Not Try Again)

<h1>Project Set-Up</h1>
Now We Need To Make Some small Changes In Our Project

 * Here I will Not Show the Mysql Set-up with That Project, I will Show more in another Section

Now Follow The Following Steps One by one

- Goto 'manage.py' File Location In Your Django Project And Create Two new File called 'docker-compose.yml', Another one file should have a '.env' extension and You Can use Any Name For that (ex: mydb.env) and Write The Following Code Into That.

```bash
services:
  mysql:
    image: "mysql"
    expose:
      - "3306"
    restart: always
    env_file:
      mydb.env
    volumes:
      - /var/lib/<projecty_name>:/var/lib/mysql
  mydjdno:
    build: .
    command: uwsgi --ini /code/uwsgi/uwsgi.ini
    volumes:
      - .:/code
    depends_on:
      - mysql
```
In The "env_file" Section Write your .env file name (Kindly Properly Maintain Spaces and Tab in yml files)


Now in The Following Code, You Just Need To Make Small Changes if first part is a Volumes part There are Two Locations joined with ":" we need to make Chnages only in The First Part You Can Give Any Location You Want Or You Can Use The Same location Which I have Written There, Just Change <projecty_name> with Your Project Name.
In my case My project Name is "subho".

```bash
//Rest of code

volumes:
- /var/lib/subho:/var/lib/mysql

// Rest of code
```
** If your Project has more Requirements Like Redis, Rabbitmq, etc. You Must Include Them in the yml file.

- Now In The Same File Location Create a New File "Dockerfile" (with no extension) And Write Down The Following Code

```bash
FROM python:3.10.6

WORKDIR /code

RUN pip install --upgrade pip
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/

```
** In The Fist Line Of Code I Have Written The Required Version Of Python For My Project You Can Change It With Your Project Requirement 

This Code Will Copy Your Project and requirements into the Container and install all requirements.

- Now In The Same Location Create a folder called 'uwsgi' and withing create a file called 'uwsgi.ini' and Write Down The following Code

```bash
[uwsgi]
socket=/code/uwsgi_app.socket
chdir=/code/
module=<main app name>.wsgi:application
master=true
chmod-socket=666
uid=root
gid=root
vacuum=true
```

In The Code In the Module Section Change The <main app name> with your Main App Name (which contains wsgi.py and settings.py) after That It Should Look like that.
In MyCase my Main App Name is 'core'

```bash
[uwsgi]
socket=/code/uwsgi_app.socket
chdir=/code/
module=core.wsgi:application
master=true
chmod-socket=666
uid=root
gid=root
vacuum=true
```
- Now We Will Write Code in .env file (In mycase it's mydb.env)

```bash
MYSQL_ROOT_PASSWORD=<Enter The Root password here >
MYSQL_DATABASE=<Enter Your Database Name >
MYSQL_USER=<Enter Any User Name>
MYSQL_PASSWORD=<Password For User >
```

Example:

```bash
MYSQL_ROOT_PASSWORD=Iamroot1234
MYSQL_DATABASE=mydjangodb
MYSQL_USER=trisha
MYSQL_PASSWORD=trisha@django1234
```
# please Wait There is More
# Working On IT