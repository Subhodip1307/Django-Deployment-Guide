# Nginx SSL Setup

In this section we will learn how we can enable SSL in a site which is served by nginx

# Downloading Requirments
Run the following command 

```bash
sudo apt install certbot python3-certbot-nginx
```

# Enableing SSL
Run the following commands

```bash
sudo su
cd etc/nginx/sites-available
certbot  
```
after choose the domain name or names by providing there index number you can also provide multiple domain names indexs useing coma
after that press enter.
Next Just Restart Your Nginx

```bash
nginx -s reload
```
# SSL Validation Check

```bash
certbot certificates
```
This Command will inform you about the validation of your SSL certificates

# SSL Renewal

```bash
sudo certbot renew 
```
This Command will renew all expeired SSL certificates and after that restart your nginx

```bash
nginx -s reload
```
