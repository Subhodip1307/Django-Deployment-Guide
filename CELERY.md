# Django Deployment Using Celery
Celery is an asynchronous task queue or job queue based on distributed message passing. It is used to execute tasks in the background, such as sending emails, processing data, or generating reports. Celery is often used with Django, a Python web framework, to improve the performance and scalability of Django applications.
If You wanna Know More About it Then Do check-out [Celery-official Docs](https://docs.celeryq.dev/en/main/django/first-steps-with-django.html)



# Method 1

In This Method We Will See The Easyest Way To Set-up Celery (Not Useing docker) for production.

<h1>Basic Setup</h1> 

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

<H3>Docker Install</H3>
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
 
   ```bash
           celery -A <project name> multi start <any worker name> \
        --pidfile="$HOME/run/celery/%n.pid" \
        --logfile="$HOME/log/celery/%n%I.log"
       ```
- If You Have Made Some Changes in your tasks.py or added a new task or something like this And  you want to reflect the changes then you can run the following command

  ```bash
  celery -A <project name> multi restart <worker name> \
    --logfile="$HOME/log/celery/%n%I.log" \
    --pidfile="$HOME/run/celery/%n.pid
  ```
- And Finally, if you want to stop celery then run the following command

```bash
celery multi stopwait <worke name> --pidfile="$HOME/run/celery/%n.pid"
```
Now you are ready to use Celery and if you face any problem issue Section is open for you 

# Method 2
 Here we will see how can we depoly celery with Docker.

<H2>Step 1</H2>
In This Step We Will Create Docker Image.
In My Case I will create a docker Image Name "Docker_celery" then paste the following code there then we will understand what is written there.

```bash
FROM python:3.10.6

WORKDIR /code/

RUN pip install --upgrade pip 
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/
```
- Here I have selected python 3.10.6 you can choose another verson of python.
- Here I have selected 'code' as my working Dir.
- Here I am upgradeing the pip to it's newest version.
- As It's a python project I have my 'requirements.txt' file to install all required packages so I am moving in to docker image ( remember this file should contains all the required packages that's the project is neeed).
- Next I am runing comand to install all requirements from the txt file.
- the I am copying all the codes for project from host machine docker image.

<H2>Step 2</H2>
In this step we will write docker-compose file for our celery and message broker (rabbit Mq or redis) and for some reason you want run message broker in host machine (not recomondated) you don't need to write them in your docker-compose file.

Add the following code to your existing docker-compose code.

```bash
 msgbroker:
    image: "rabbitmq:management"
    restart: always  
    volumes:
      - /var/www/data:/var/lib/rabbitmq
celery_task:
    image: celery_image
    build:
      context: .
      dockerfile: Docker_celery
    volumes: 
      - .:/code
    restart: always  
    command: celery -A core worker --loglevel=INFO --concurrency=4 -n worker1@%h
```
- Here the first image is for the message broker for celery and the name and I am useing Rabbitmq you can also use redis.
- Always set the volumes of out your project folder.
- Next I have written code for celery and in Image section I am giving a name for celery job which is 'celery_image' which we can use in future for more celery worker for same project 
- Next in 'dockerfile' I have given docker file that we have made before and then we provided the working dir name (The dir name should be same as the Docker file made for celery)
- Now let's undersatand the command section 
 

# Working on it please wait

