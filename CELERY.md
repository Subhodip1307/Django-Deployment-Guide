# Django Deployment Using Celery
Celery is an asynchronous task queue or job queue based on distributed message passing. It is used to execute tasks in the background, such as sending emails, processing data, or generating reports. Celery is often used with Django, a Python web framework, to improve the performance and scalability of Django applications.
If You wanna Know More About it Then Do check-out [Celery-official Docs](https://docs.celeryq.dev/en/main/django/first-steps-with-django.html)

# Basic Setup

First We Gonna Make Basic Setup celery With the following steps. Example [Code]()

- Install Celery (First Activate your python virtualenv )

```bash
pip3 install django-celery
    or,
pip install django-celery 
```
- Basic Set-up With Your Django Project
  In this Step, You Need To create a Python file called 'celery' In your Main App (where "settings.py" is present) and write The following Code.

  **Replace The <project name> with Your Actual Project Name

```bash
import os
from celery import Celery
#Replace The "<project name>" with Your Actual Project Name
os.environ.setdefault('DJANGO_SETTINGS_MODULE', '<project name>.settings')
app = Celery('<project name>')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
```
- Now Create A New File called tasks.py To Your App (Where 'models.py' file is present).

  ** Then Write your Task with "@shared_task" decorator like the example Code

```bash
from celery import shared_task

@shared_task
def thanks_for_comming():
    return "Please Give a start"

@shared_task
def Onemore():
    pass
    # Do anything

@shared_task
def add(x, y):
    return x + y
```
** This is some example code you can write your own 

#  Message Broker Installation and Setup
  * There are Two Famous Options for celery to use as Message Broker Redis & rabbitmq, I will show Both Methods one by one 
<H2>Redis</H2>
There Are Two options to install Redis on your server

- Direct install to your system
- Install and Use with docker

* Both Options Will Work the Same but I would suggest using it with Docker.

<H3>Direct Install</H3>

To Install it Directly You Can follow [Redis Offical Docs](https://redis.io/docs/install/install-redis/install-redis-on-linux/)

<H3>Docker Install</H3>
* You Should Have Set-up Docker in your system
- Create a file called "docker-compose.yml" (Where You manage.py is Present)

  * IF You Want You Can Create The File In Other Place
    
- Write The Following Code In the Yml File

```bash
version: '3.7'

services:
  redis:
    image: redis:latest
    restart: always
    ports:
      - "6379:6379"
```  
* If You want you can change the ports or set up another network type

<h2> Django Set-up For Redis</h2>
Add The Following Line to your settings.py file

```bash
CELERY_BROKER_URL = 'redis://127.0.0.1:6379/0'
```
<H2>Rabbitmq</H2>
There Are Two options to install Redis on your server

- Direct install to your system
- Install and Use with docker

* Both Options Will Work the Same but I would suggest using it with Docker.

<H3>Direct Install</H3>

To Install it Directly You Can follow [RabbitMQ Offical Docs]([https://redis.io/docs/install/install-redis/install-redis-on-linux/](https://www.rabbitmq.com/docs/install-debian#apt-cloudsmith)https://www.rabbitmq.com/docs/install-debian#apt-cloudsmith)

H3>Docker Install</H3>
* You Should Have Set-up Docker in your system
- Create a file called "docker-compose.yml" (Where You manage.py is Present)

  * IF You Want You Can Create The File In Other Place
    
- Write The Following Code In the Yml File

```bash
version: '3.7'

services:
  msgbroker:
    image: "rabbitmq:management"
    ports: 
      - "5672:5672"
    restart: always  
    volumes:
      - ./rabbitmq-data:/var/lib/rabbitmq 
```  
* If You want you can change the ports or set up another network type or volumes or service name etc.

<h2> Django Set-up For RabbitMQ</h2>
Add The Following Line to your settings.py file

```bash
import os
CELERY_BROKER_URL  = os.environ.get('RABBITMQ_URL', 'amqp://guest:guest@localhost:5672/')
```
# Depolying Celery For Production
Now We will Depoly Celery For Production in a Few Following Steps

- Check If Your Celery is Configured Perfectly or not

  if you use Docker for your message Brocker then Run the following Command (Run The command where your "docker-compose.yml" file is present)

  ```bash
  docker compose up -d
      or
  docker-compose up -d
  ```
  It will Run Your Service of The Message Broker, Now Activate for virtualenv and Run  the following Command

  ```bash
  celery -A <project name> wroker -l info
  ```
  **Replace The <project name> With Your Project Name
  
  After running this Command if you not facing any Errors like "consumer: Cannot connect to ..." then you are ready to go further

- Deploying
  * For This Method I should Have root privileges (if you are following the guide from the beginning then you no need to worry)
    - Run The following Command

      ```bash
          sudo su
      ```
      * now you have root privileges ,This Time Run The final Command

       ```bash
           celery -A <project name> multi start <any worker name> \
        --pidfile="$HOME/run/celery/%n.pid" \
        --logfile="$HOME/log/celery/%n%I.log"
       ```
       **Replace The <project name> With Your Project Name and  replace it <any worker name> with any name you want to give to your worker

Now You have Successfully Depolyed Your Celery, To check it Run the Following Command (should Have Virtualenv activated).

```bash
celery status
```
** After Running This command You will See all running nodes  for your project, Now it's Ready to use

Now Read The Following points Carefully.

- If you want To start Another node Just Run this command again with another worker name
- 
   ```bash
           celery -A <project name> multi start <any worker name> \
        --pidfile="$HOME/run/celery/%n.pid" \
        --logfile="$HOME/log/celery/%n%I.log"
       ```
