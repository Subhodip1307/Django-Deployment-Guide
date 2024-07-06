# Django Deployment With Nginx-uwsgi with docker
Here We will Depoly Our django App With Nginx and Uwsgi in a Docker Container ( I am asumeing That You Have At Least Basic UnderStanding Of Docker)

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
  mysqldb:
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
CMD ["python3" "manage.py", "colletcstatic"]
```
** In The Fist Line Of Code I Have Written The Required Version Of Python For My Project You Can Change It With Your Project Requirement 

** I am assumeing that you have set path or static Root in django settings.py

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
# Django Setting Set-Up
Now We Need To make Small changes in Our Django settings
** Here I will Not include Normal Settins Like Debug,key etc.

```bash
  DATABASES = {
        "default": {
            "ENGINE": "django.db.backends.mysql",
            "NAME": "mydjangodb",
            "USER": "trisha",
            "PASSWORD":"trisha@django1234",
            "HOST": "mysqldb",
            "PORT": 3306 ,
        }
```
As I am Mentioned in My docker-compose.yml file that I am useing a service call "mysqldb" for MYSQL database so I have filled all the database related credentials one by one from the  "mydb.env" and in the host name I have mentiond the service name.

** If you More Requirements Like rabbitmq or something like that then replace the host name with the service name of that requirements and enter expose port or deafult port of that Service

** I Hope You WIll Change Some Simpel Settings On Your OWn Like Makeing The Debug=True To Debug=False,Allowed HOST ect (But I Would Suggest You Not To Make The Debug=True untill You Successfully Depoly Your Project).

** You Can also Use Docker environment Variables

# Nginx Set-up
 
 **I am Keeping The Nginx Out Of My Docker Container Due Making This More Simple And Depoly More Than One Application In A Singel Server .
 
 Now Run The Follwing Command To Change Your Dir.
 
```bash
cd /etc/nginx/sites-available/
```
Now Create a File With Your Domain Name (ex: In My Case My Domain is Subho.com)

```bash
syntax: nano domain
example: nano  Subho.com
```
Now Copy The Following Code Pasted it in Your Newly Created Nginx file File

```bash
upstream <app_name> {
  server unix:<path to Your Project Folder>/uwsgi_app.socket;
}

server {
 listen 80;
 server_name www.<domain_name> <domain_name>;
 location / {
    include /etc/nginx/uwsgi_params;
    uwsgi_pass  <app_name>;
    uwsgi_read_timeout 70s;
    }
 location /static/ {
    alias <location to your static files>;
  }
location /media/ {
  alias <location to your Media files>;
}
}
```
Now Let's Edit The Code in Few Steps

- In First Line There is a line 'upstream <app_name>' here replace the <app_name> with any name you want ( it's suggested to use your project name , in My Case it will be 'myproject'), and then in The next Line replace the '<path to Your Project Folder' with your project location (in my case it's '/var/www/myproject')

```bash

  upstream myproject {
  server unix:/var/www/myproject/uwsgi_app.socket;
  }

```
- Now in the 'server_name' add your domain name (ex: in my case it's Subho.com)

```bash
 server_name www.Subho.com Subho.com;
```
- Now  in the 'uwsgi_pass' Section change the  <app_name> with the Name you given at first (ex: In my case it's 'myproject')

```bash
uwsgi_pass myproject
```
- Now change the value 'uwsgi_read_timeout' around 70s to 100s (The uwsgi_read_timeout directive in NGINX sets the amount of time upstream waits for a uwsgi process to send response data. The default value is 60 seconds) 

- Now There is two more location section one is for static files and another one is media file now write the location according to your project

```bash
location /static/ {
alias /var/www/myproject/static/;
}
location /media/ {
alias /var/www/myproject/media/;
}
```
**if You Don't have any media  file or static file or both of them you can simpley remove the location form your file

Now we are done, finally the file should look like (according to my project name,location etc.)

```bash
upstream myproject {
  server unix:/var/www/myproject/uwsgi_app.socket;
}

server {
  listen 80;
  server_name www.subho.com Subho.com
  location / {
    include /etc/nginx/uwsgi_params;
    uwsgi_pass myproject;
    uwsgi_read_timeout 100s;
    }
  location /static/ {
  alias /var/www/myproject/static/;
  }
  location /media/ {
  alias /var/www/myproject/media/;
  }

}

```

# Start Docker

Till now we have done setting up docker-nginx and database now we are ready to start the docker deamon for that do to your project dir and run the following command

```bash
docker-compose up -d
nginx -s reload
```
** wait for a few seconds and keep checking the logs after few time go to your browser and search for the domain and you will able to see your website (with http not https) if you encounter any error like 500 then go and resart your uwsgi container 
in my case it's name is 'mydjdno' so the command will be

```bash
docker-compose restart mydjdno
```
if the issue still not resolved try checking 

```bash
docker-compose ps -a
```
and check which one is causeing issue and try to fix that by restarting that or check it's logs,
if you have another service like celery dont forgot to restart them also. now in final step we will learn to activate SSL in our site 



