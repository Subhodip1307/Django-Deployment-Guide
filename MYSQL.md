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




  
- Go To your Project Dirctory and open settings.py make this changes and go database section ( where code is written for database)
   
```bash
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```


- Save The file and come where is your 'manage.py' file and create a folder

  ```bash
  mkdir sqlitedb
  ```
- Then move Your database to that folder

    ```bash
    mv db.sqlite3 sqlitedb
    ```
- Now we are almost done now just need to give read and write permission

```bash
  sudo chown -R www-data:www-data sqlitedb
  sudo chmod 775 sqlitedb
  sudo chmod 664 sqlitedb/db.sqlite3
```
- Now are done just restart the server and it will work

```bash
sudo service apache2 restart
```
