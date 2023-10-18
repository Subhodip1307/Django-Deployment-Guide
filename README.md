# Django-Hosting-full-guide-
Welcome to this repo here you will learn how to host your django site in vps or any linux server.
Most of the django devoloper faces problem to host there site after createting there dajngo Project so no need to worry any more in this Guide you will learn how to host your site in any vps.
So we let's look at list and here I mention that what we gonna use.

<ol>
    <li>python</li>
    <li>Apache Webserver</li>
    <li>My SQL Database</li>
    <li>VPS (ubuntu)</li>
  </ol>

If you noticed clearly in this list I haven't metion django and it's version ,because it's does't metter which versiob you useing of dajngo and python.
And I am useing MySQl database you any Database The process will remain same except sqlite ( we will discuss it letter)

# Project Overview

In this section we will see a quice over view that how your project should be.
first look at the project files.
suppose I have created project like this
```bash
  python -m django startproject myproject
              or
  django-admin startproject myproject
```
I have also created an app
```bash
python manage.py startapp app1

```
 
Now lets look at the Structure of our project

```bash
    myproject(the project folder name)
    |_myproject
    |_app1
        |_template
        |_static
    |_myvnv(virtual env)
    |_manage.py

```
In this structure You can see that I have put my tempalted and static  folder in a app and your project structure should be like this.
If You are unable to understand or comfuse that how use this strcutre or something like this then don't worry I will attach a sample project with this repo.

# Basic VPS Configuration
In this section we will configure our VPS and donwload all dependensis for our project and we will also create a user.

<h2>Createing a User in VPS</h2>
so in first step we need to login in our vps with ssh key or ip . Here I will use ip and username to login

```bash
ssh root@your_server_ip
```
example:
suppose my VPS ip is :-192.168.0.1 and by default the username will be root
```bash
ssh root@192.168.0.1
```
And Then give your password
Now we are successfully loged in our VPS, now We will create a user

```bash
adduser <username>
```
example:- suppose I wanna create a user name 'trisha'

```bash
adduser trisha
```

now we will give the user subho privilages so in our case our user is 'trisha'

```bash
usermod -aG sudo trisha
```
<h2>Now we will set-up our Firewall</h2>

Just run the command

```bash
ufw allow OpenSSH
ufw enable
```

Now all Done now exit from the vps

```bash
exit
```

#Uploading Django Project To our VPS

There I will discuss two methods to upload your project to your vps one is with GitHub and another one is from your Local System . We will look at both.

<h2>With Github</h2>
It's very easy to upload your project with github just relogin in vps with new username and password.
,in our case our new user name is 'trisha' and our ip is '192.168.0.1' so to login we need to run that

```bash
    ssh trisha@192.168.0.1
```
Now we are in our Home dirctory now just use gitclone url and clone your project

```bash
    git clone <your project github link>
```
And then it may ask  for authentication for your github.
first it will ask the github username and the password. For That situation we need to create a accesstoken from github , You can watch videos in youtube for that or I wll discuss it very soon.

Now let's see does our project successfully cloned in our VPS or not for that use this command 

```bash
    ls
```
After that if you able to see your project name then congratulation . Now let's move on another method to upload your project 

<h2>Upload From Local</h2>
We will upload the projcet from our localsystem with scp, so for that keep your project in your desktop and open terminal there (Don't forget to make it zip)
now run this command.

```bash
scp -P Port_number Source_File_Path Destination_Path
```
example :- Nomrally we access our VPS with port 22 and our projectname is myproject.zip (I have made it zip) and Destination_Path will be our username and ip 

```bash
scp -P 22 myproject.zip trisha@192.168.0.1:
```
Now it will take sometime and then loging in your vps with your username in our case this is 'trisha'

```bash
    ssh trisha@192.168.0.1
```
Now let's see does our project successfully cloned in our VPS or not for that use this command 

```bash
    ls
```
If you see your project name then You have successfully done now let's unzip the project file inoder to do that we need to install unzip .

```bash
sudo apt install unzip
```
now we have downloaded unzip now let's unzip our project

```bash
unzip <your projectname>.zip
```
example:- In our case our folder name is myproject.zip

```bash
unzip myproject.zip
```

# Downloading Requirments
so let's download our Requirments, before that update your system

```bash
sudo apt update
sudo apt upgrade
```

Run This commands one by one to Download all requirments 

```bash
sudo apt install apache2
sudo apt install mysql-server
sudo apt install python
sudo apt install libapache2-mod-wsgi-py3
sudo apt install python3-pip
sudo pip install virtualenv
```
Now we have donwloaded everything now let's check everyting is fine or not.
Run This follwing commands if we are not getting any error then Everthing is fine

<ul>
  <li>
  Checking Python

      
      python3
      
          
  </li>
  <li>
  Checking Apache

    sudo service apache2 status
  </li>
<li>
    Checking Mysql

    mysql
</li>
</ul>
Now type 'exit' to exit from mysql

Now we are done next we will deploy our project

# Deploying Project

Now we need to move our project file to a 'www' dirctory to do that 

```bash
sudo mv project_folder_name /var/www/
```
example:- In our case our project name is 'myproject' 

```bash
sudo mv myproject  /var/www/
```
now let's go to the localtion and check if the project moved successfully or not

```bash
cd /var/www/
ls
```
After that you will be able to see your project in the directory now let's move to the next step.
Now go your project and active your virtual env and download all requirments . For now I hope that you know how to do that and skipping it for currently and moveing to the next step.

Now suppose I have a domain which I have pointed to my vps Ip and the domain is- 'subho.com' so you will have create a 
config file for apahce with name of your domain.
Don't forgot to use '.conf' extension with your config file

```bash
syntax: sudo nano /etc/apache2/sites-available/your_domain.conf
example: sudo nano /etc/apache2/sites-available/subho.com.conf
```
 After runing this command a code editor will open to you and paste the following command there (You will need to make changes here to use it , I will discuss it after the code)

 ```bash
<VirtualHost *:80>
    ServerName www.example.com
    ServerAdmin contact@example.com
    DocumentRoot /var/www/project_folder_name
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    
    Alias /static /var/www/project_folder_name/static
    <Directory /var/www/project_folder_name/static>
        Require all granted
    </Directory>
    
    Alias /media /var/www/project_folder_name/media
    <Directory /var/www/project_folder_name/media>
        Require all granted
    </Directory>
    
    <Directory /var/www/project_folder_name/Inner_project_folder_name>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>
    
    WSGIDaemonProcess project_folder_name python-home=/var/www/project_folder_name/virtualenv_name python-path=/var/www/project_folder_name
    WSGIProcessGroup project_folder_name
    WSGIScriptAlias /  /var/www/project_folder_name/inner_project_folder_name/wsgi.py
</VirtualHost>

```
<b>Explanation</b>
now let's unser stand how to use the whole code so make changes one by one.

<u>Step 1:</u>

```bash
ServerName www.example.com
```
Here You need to Put your doamin in my case it's 'subho.com'

```bash
ServerName www.subho.com
```
<u>Step 2:</u>

```bash
ServerAdmin contact@example.com
```
in this section you need to put your email in my case it is 'subho1307@gmail.com'

```bash
ServerAdmin subho1307@gmail.com
```
Now let me tell you the use of the email . If your webserver or user faced any problem it will show your email to the user
and tell them to contact you to fix the problem.

<u>Step 3:</u>

```bash
DocumentRoot /var/www/project_folder_name
Alias /static /var/www/project_folder_name/static
<Directory /var/www/project_folder_name/static>
Alias /media /var/www/project_folder_name/media
<Directory /var/www/project_folder_name/media>
```
Here I am showing some different line code and here is a keyword that is 'project_folder_name' and there some more keyword
in your just change it with your projectname in my case this is 'myproject' so I will replace all 'project_folder_name' with 'myproject'. After There YOu will find one more keyword that will be 'inner_project_folder_name' there you will have 
provide that folder name which contains 'settings.py' in my case that is also same 'myproject' so I will replace all of them with that.

<u>Step 4 (final):</u>

```bash
    WSGIDaemonProcess project_folder_name python-home=/var/www/project_folder_name/virtualenv_name python-path=/var/www/project_folder_name
```
in this line of code here is a keyword call 'virtualenv_name' just replace it with your virtualenv name in my case it's 
'myvnv' . After Making all changes it should look like this .

 ```bash
<VirtualHost *:80>
    ServerName www.subho.com
    ServerAdmin subho1307@gmail.com
    DocumentRoot /var/www/myproject
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    
    Alias /static /var/www/myproject/static
    <Directory /var/www/myproject/static>
        Require all granted
    </Directory>
    
    Alias /media /var/www/myproject/media
    <Directory /var/www/myproject/media>
        Require all granted
    </Directory>
    
    <Directory /var/www/myproject/myproject>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>
    
    WSGIDaemonProcess myproject python-home=/var/www/myproject/myvnv python-path=/var/www/myproject
    WSGIProcessGroup myproject
    WSGIScriptAlias /  /var/www/myproject/myproject/wsgi.py
</VirtualHost>

```
<h1>Still working on It</h1>
