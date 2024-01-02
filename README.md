# Django-Hosting-full-guide-
Welcome to this repo here you will learn how to host your django site in vps or any linux server.
Most of the django devoloper faces problem to host there site after createting there dajngo Project so no need to worry any more in this Guide you will learn how to host your site in any vps in any web-server.
I will show here all methods to depoly your django project.
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
    |_requirments.txt

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

