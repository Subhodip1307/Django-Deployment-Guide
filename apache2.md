# Djano Depolyment With Apache2
 Here we will see how can we depoly our django app with apache2 and after that we will see how to configure data base for our site then look over other djano framworks (celery, django channels) and learn how to depoly them 

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

First we need to set up Firewall permission to apache, just copy the command and pasted it.

```bash
ufw allow "Apache Full"
```

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

# Downloading Project Requirments
In order to do that you need to activate your virtualenv

```bash
source myvnv/bin/activate
```
Now we can download required python modules in my case I have requirments.txt (if you don't have requirments.txt then you download one by one)
```bash
pip install -r requirments.txt
```
After donwloading everything you Check it by this command -:

```bash
pip list
```
We have successfully downloaded all requirments now deactivate the virtual env and move to depolyment stage

```bash
    deactivate
```
# Media File Permission
If You have media files then you need to go thorugh the following steps

- Adding User to 'www-data' group (www-data is a user and group that the service httpd (Apache) uses to run on a system)
- In my case my username is trisha
  
```bash
sudo usermod -a -G www-data trisha
 ```

- Giving Permission To Media Folder ( I am gussing That Your Media folder name is Media and Run This caommnd Where your media file is)

 ```bash
sudo chown -R www-data:www-data media
```

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
Now we are almost done , next we will check that your conf code is correct or not and then we will active it , so in order to do that run this 

``` bash
sudo apache2ctl configtest
```
and if everything is fine then we will activeate this conf file.

```bash
sudo a2ensite your_domain.conf
```
in my case my conf file name was 'subho.com.conf'

```bash
sudo a2ensite subho.com.conf
```

Now we need to make some more small chnages then we can depoly our project so to order to do that run this following command

```bash
cd /etc/apache2
sudo nano apache2.conf
```
and then go at the bottom of the file and then paste this :-

```bash
WSGIApplicationGroup %{GLOBAL}
```
Then exit from the nano editor after saveing the changes ( I hope you know how to do that ).
Now restart Apache server
```bash
sudo service apache2 restart
```
Now We are all done with apache , and now let's look at our django project and make some small changes in that project with some following steps

<h3>Step 1:</h3>
Go to our settings.py file and open it in nano editor ( or any other text editor)

```bash
cd /var/www/miniblog/miniblog
sudo nano sudo nano settings.py
```
<h3>Step 2:</h3>
Make The following Changes

```bash
ALLOWED_HOST = ["your_domain"]
```
<ul>
    <li>Type Your Domain Here in my case this is 'subho.com' so it should look like this</li>
        
        
        ALLOWED_HOST = ["subho.com","www.subho.com"]
</ul>
<ul>
    <li>
       I have't any domain then write your IP address of your VPS here ( type 'ifconfig' in your terminal of vps to know our ip) 
    </li>

    
            ALLOWED_HOST = ["192.168.0.1"]
            
</ul>

<h3>Step 3:</h3>
Write this in to Your settings.py

```bash
DEBUG = False
STATIC_ROOT = BASE_DIR / 'static'
```
and then save and exit from the text editor 

<h3>Step 4:</h3>
Now ativate your virtual env and collect staticfiles

```bash
source myvnv/bin/activate
```
and then run this

```bash
python manage.py collectstatic
```
If you get any error after running this command you make a googel search for it or you can message me in this repo.

After all of that we need to restart our apache server and then we can see that our site is on live.

to do that run this command 

```bash
sudo service apache2 restart

```
After That you can open the web address and you will able to see your site.
But currently I will run on http not https so we need to enable SSL for our site , and keep in mind you can't work in your site due to uncifgured data base ,
so will do everythig one by one . First let's active SSL

# Enableing SSL
Here We will enable SSL for our site in few easy and simple steps

- First We need to install "certbot" to do that run this follwing command

  ```bash
  apt install certbot python3-certbot-apache
  ```

- Open Your Site Config file of apache ( where We written code for our site)

```bash
syntax: sudo nano /etc/apache2/sites-available/your_domain.conf
example: sudo nano /etc/apache2/sites-available/subho.com.conf
```
- Now comment The Last 3 Lines by useing '#' (excpet '</VirtualHost>')

  ```bash
#WSGIDaemonProcess myproject python-home=/var/www/myproject/myvnv python-path=/var/www/myproject
#WSGIProcessGroup myproject
#WSGIScriptAlias /  /var/www/myproject/myproject/wsgi.py
  ```
- Verify Web Server Ports are Open and Allowed through Firewall

```bash
ufw status verbose
```
If you Don't see apache name after running this command the run the follwing command then run the command

```bash
sudo ufw allow "Apache Full
```
- Now Run This command and you will see all domains that are pointed to your vps chose or multipule. 
 
 ```bash
certbot --apache
```
- After Doing That Open your site conf file Again And uncomment The lines
- Next There will be another new file with almost same name of your conf file (end with '-le-ssl.conf') open that file in text editor .
- Uncommnet the same 3 lines and change your project name or add any extra letters in your project name (ex: my project name was 'myproject' and I made it 'myprojects')

```bash
WSGIDaemonProcess myprojects python-home=/var/www/myproject/myvnv python-path=/var/www/myproject
WSGIProcessGroup myprojects
WSGIScriptAlias /  /var/www/myproject/myproject/wsgi.py
 ```
- Now Run This following command

```bash
cd /etc/apache2/sites-enabled
```

- After That use 'ls' Command You will The  same files which you just edited
- You will Need to make same changes What you have done here (uncommeting the last 3 lines in first file and in the second file uncomminting the 3 lines and changeing the project name)
- Now Restart Your Apache To see the changes
  ```bash
  sudo service apache2 restart
  ```
  ** Now You Have enabled SSL To your Site ,Now You need to configure Data base To use Your site properly Now Chose any Data Base Opetion From Following to do that

  - Sqlite (working on it)
  - Mysql/Mysql-server (working on it)

  
