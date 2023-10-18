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
    |_manage.py

```
In this structure You can see that I have put my tempalted and static  folder in a app and your project structure should be like this.
If You are unable to understand or comfuse that how use this strcutre or something like this then don't worry I will attach a sample project with this repo.
