# Django Deployment Useing Sqlite
SQLite is a serverless, relational database management system that is open-source and in-memory. It is the most widely used SQL database engine in the world. 
And This Database is default Database of Django and its very simple to use. 
** If you are useing This Database and want a daily backup of your data in a fun way this you check my [DataBase-Backup](https://github.com/Subhodip1307/DataBase-Backup) repo.
following rules will tell you how to setup Sqlite as in production:-
- Go To your Project Dirctory and open settings.py make this changes
  
** if you face 'permission denied' in any command then use sudo with that command 

```bash
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'sqlitedb/db.sqlite3',
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
