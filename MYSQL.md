# Django Deployment Useing MYSQl/MYSQL-SERVER
SMySQL is a relational database management system (RDBMS) that allows users to store, manage, and retrieve structured data. It is based on structured query language (SQL) and is developed by Oracle.
And Its very easy to use and integrate with django.

** If you are useing This Database and want a daily backup of your data in a fun way this you check my [DataBase-Backup](https://github.com/Subhodip1307/DataBase-Backup) repo.

following rules will tell you how to setup Sqlite as in production:-
  ** remember I will show here the simplest way to set-up and enableing backup for your project ( I have given a link to repo for backup your database)

# Installing and setup MYSQL
 ** The following methods are only for debian linux [click here](https://en.wikipedia.org/wiki/List_of_Linux_distributions) To know more about it.

- Updateing system and Installing Mysql

```bash
sudo apt update
sudo apt install mysql-server
```
- Checking the installation 

```bash
sudo systemctl status  mysql
```
** After running this command if you don't get any error thats mean you have successfully installed mysql

- Start Mysql service

```bash
sudo systemctl start mysql
sudo systemctl enable mysql
```
- Opening The Mysql shell (For The First Time)

```bash
sudo mysql
```
- Seting Password For Our Root user (run the following command in mysql shell)

```bash
ALTER USER 'root'@'localhost' IDENTIFIED  BY '<your password>';
```

- Now are done let check the password for that

```bash
exit
```
** This command will exit you from Mysql shell

```bash
mysql -u <username> -p
```
** In our case usename is 'root' and after this command it will ask for password , type the password that we have set recently.

** If The username and password is correct you will again enter in mysql shell

# Createing New database
here we will create new database for django project with following easy steps.

- If you are not in your mysql shell then loging there (if You are already in mysql then skip this step)

```bash
mysql -u <username> -p
```
- Viewing All data bases

```bash
show databases;
```
- Createing A New Database

```bash
create database <database name> ;
``` 
** provide any name as you want and it will create database with that name , 
example:-

```bash
create database subhodip ;
```
- To check Your Database

```bash
show databases;
```
** You will able to see your database after this command

** Now we have created Our database

# CREATING MYSQL USER AND GRANTING PRIVILEGES

** It's very Important part so please follow each step carefully

- Creating New User

```bash
CREATE USER '<username>'@'localhost' IDENTIFIED BY '<password>';
```
** Provide any user name (except 'root') and password  or you can put your django project name

example:

```bash
CREATE USER 'mydjango'@'localhost' IDENTIFIED BY 'subhotrisha@12';
```
** After That I will have an user whose name is 'mydjango' and password is 'subhotrisha@12'

- Check If The User created successfully or not 

```bash
SELECT user FROM mysql. user;
```

** After This command You will able to see all usernames list of mysql (if you don't see your created username the try again)

- Granting Privileges on Database

I hope You Remember That we have created a database (In my case my database name was 'subhodip' and myusername was 'mydjango')
Run The Following command to Grant Privilegs

example:

```bash
GRANT ALL PRIVILEGES ON <database name>.*  TO '<username>'@'localhost';
```
Syntax:

```bash
GRANT ALL PRIVILEGES ON subhodip.*  TO 'mydjango'@'localhost';
```
** Now We are Almost done and the only last thing is to check the Privileges of the user 

example:

```bash
SHOW GRANTS FOR '<user>'@'localhost'; 
```
Syntax:

```bash
SHOW GRANTS FOR 'mydjango'@'localhost'; 
```
** Now We are all done just run the following command and move tho your project directory

```bash
exit
```

# Djano-Setup

- Go To your Project Dirctory and open settings.py make this changes and go database section ( where code is written for database) and you will able to see this (if you didn't change it before)
   
```bash
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
- Replace The Old Database code of settings.py with this

```bash
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": "<database name>",
        "USER": "<user name>",
        "PASSWORD": "<password>",
        "HOST": "127.0.0.1",
        "PORT": "3306",
    }
}
```
- Enter Your database name,User name and password (in my case Database name was 'subhodip' and username was 'mydjango' and the password is 'subhotrisha@12')

** suggestion :- Don't user root user in your project , for this reason we created a new user 

After that the code should look like this 

```bash
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": "subhodip",
        "USER": "mydjango",
        "PASSWORD": "subhotrisha@12",
        "HOST": "127.0.0.1",
        "PORT": "3306",
    }
}
```
- Save The changes and exit

- Activate Your python virtualenv ( my env name was myvnv)

```bash
source myvnv/bin/activate
```
- Run The Following Command To Download Required package

```bash
pip3 install mysqlclient
```
- Run Migrations

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```
** If you followed all the steps properly then it will migrate and no error will be shown

- Refresh your web-site open the Dirctory where 'settings.py' file is ,

Run the following command

```bash
ls
```
If you see any file call 'wsgi.py' then run the following command or you are in wrong Dirctory

```bash
sudo touch wsgi.py
```
** We should not restart your webserver may effect on webservices running in your vps so run this command each time you make any changes to your site to show the changes

<h1>congratulations Now Site is Ready To Use</h1>

