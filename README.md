Udacity-Linux-Server-Configuration

About

This is the Udacity project 5 about the Configuring the Linux the server.

Server Details
Server IP Address 13.126.182.198

Hosted site Url http://13.126.182.198.xip.io

How to connect as grader:

save private key provided in your local machine and run the following command

ssh -i path/to/privatekey -p 2200 grader@13.126.182.198

private key:
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA1seajVd7jNaRs5TgLuQqbaSiSm+dwd96fUK/CHvOJcZXAUAj
QLn2ZQZ5SxgapdK6hxoOb4jdA+eeYBthDiNALHzR2IwwCg6AqnHyZIB1KO2bnO3A
G0PS6n98hEqMtvWljjfQL0ov8qigMOhFJ6GM4313LRtOcrdW8QsK6RcFcAkblSeU
s/116vtec8nwb/3T6/HX2SHx9nRCKWHQ0ZswDPAtMfN0FeRcVlFxnzIwCfsOdmXO
B+y41OQnDslaQdsYqxgERx7wjEFZy7bha4EC49NduEaIE5a2c4Y7YNRHhAd9GHAR
Z8LVOxI3kAKcR00h1b2CGMy5CRzInAedNKEWyQIDAQABAoIBADNz1d6OWpa+wGDZ
BWu2VUO28PoMCyrqsplXaBRMNHJwNV+jUc6rvg8todkPiTK4bN74qwSlMO1Ci3pS
lMmiQDloHY1W6BtApiou6faRn3+SjHjnq+HcOabbq6S1h0g9SM/tJv5tM1dadiXB
Pu/jj7Df2bEAnaZ1rWpJTu/QCAlydMPOR1lb1zlqQQojKdPpj2Q7EE1c2lmqAKJz
QcjjFAmR6xo9Vm+VfxecMZWMskV0B/PArhl+sx0EIusPKxslCb09e2JUC7gb3Q6/
1isfa6z9YIswsliOeLdsAYTeBiKDyteLMDvHiBCr0vwtSg/A5dtmWyanW9NaqvNZ
cUgbiEECgYEA609aXmVkw0qnggVKLQZ9HyrM/ufjPfvpapn/phZMF5HWf7g6rw3w
w/aLyeXyd77uCUWU8QnnOLHKTnaCrQKvv3QvNUSFsntDJ9JV4D7cV1TLopya4tsh
URy+/2Vu6jV1wJR6VOHvi0eOYkswCnqMBjNk1Xg1Q2EPJU+VeDa4XZUCgYEA6aoh
TFu4MOCdAExA9Ed3Y/qyHJJTpO+1p/Ch83xKH6USbUslOXBf4wlMUjAc9YCgZrPW
o0y7hx7fqxUUKK0RT2TWzANTSkKzGscDIkJm28Bztw4VeicpQotWDqNJh9LPWZVG
roRDikH63ttJkJvapimuNIkSCcx45FuznjdKv2UCgYEAv33bR9hxsK/PM3NEkvGl
3zhAjOx+tFGN+Y+LSUj58XBgQ53UO+M3XPIFfm9f62z4X5k9hQ6PGUcuIL42x77Y
8RAG3u19c+r1krGL6yqcu4EpGpMhRJ4ZNd4T3NlZ8sVAp1DtYKhg/VJlH76aQNzL
mLw4QbRKfCO/ZJioRaUaUiUCgYEAvaHP4ktxgWFYqXw5Hsa9Mwuq7xsl/O55i7Dz
jkppUaNDACYDMMltWDEcmnrnlaptAsyiveaxLmi09wBlWtmR+dAJropoVxUoi+vF
NFGVbnSStJYeggM0LggssDZ+n1dL5hUKxukacyM2+RQYcN67pSygb4xqcj9aQWHW
tOmEpS0CgYAc+rn+H6QJpe5+mDhuys4z9wTYN0ZgaVi14hHOkadMdGhv0ugYOr5s
JLMDoUIQfaZIXklAO/E1DKQwpj4hYiOrd35ln/uezNnRd85Ah4vJtiAga5W5NsoN
ysSuyCYz93fQBn54SJb7tODN3tRTkjGppm0o0XXbAxhatSrvU3g7Tw==
-----END RSA PRIVATE KEY-----

  
Configuring Linux Server
Updating all packages
sudo apt-get update
sudo apt-get upgrade
Creating grader User:
sudo adduser grader
This will add new user

sudo nano /etc/sudoers
Below the Root user append the following line

grader  ALL=(ALL:ALL) ALL
This will grant sudo permission to grader

Creating a ssh key pair for grader
On your local machine in terminal/command prompt

ssh-keygen
This will generate public and private ssh keys which is saved to .ssh folder

Then in your virtual machine change to newly created user

su - grader
Create a new directory .ssh and new file authorized_keys in that directory

mkdir .ssh
sudo nano .ssh/authorized_keys
Copy the public key with .pub extension to authorized_keys and save the file

chmod 700 .ssh
chmod 644 .ssh/authorized_keys
700 will give read write and execute permission to user.
644 prevent other user from writting in to file. Then restart ssh server
service ssh restart
Now from your log in to grader with private key generated

ssh -i .ssh/id_rsa grader@ipaddress 
Changing the ssh port to 2200
sudo nano /etc/ssh/sshd_config
Change port 22 to port 2200

Restart the ssh server

service ssh restart
Note: Before Logging using ssh add custom TCP port 2200 under lightsaail firewall in networking tab in lightsail instance console

Now Login using command like this

ssh -i .ssh/id_rsa -p 2200 grader@ipaddress
Disabling ssh login as root
sudo nano /etc/ssh/sshd_config

make change PermitRootLogin no

Configurating Ufw firewall
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/udp
sudo ufw enable
This will allow all required ports and enables the ufw

After that

sudo ufw status
It will display all allowed ports

Changing time Zone
sudo dpkg-reconfigure tzdata

select none from list and then select utc.

Installing Apache2
In terminal

sudo apt-get install apache2

Now mod_wsgi

sudo apt-get install python-setuptools libapache2-mod-wsgi

Enable mod_wsgi

sudo a2enmod wsgi

Setting up your flask application to work with apache2
Creating a flask app

In /var/www directory create a new folder sudo mkdir FlaskApp

Install git

sudo apt-get install git

move to the FlaskApp cd FlaskApp

In that direcory clone your github repository

sudo git clone https://github.com/username/itemcatalog.git

Rename your repository to FlaskApp

Then rename your project file to __init__.py

Error : While accesssing the client_secrets.json file

PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))
json_url = os.path.join(PROJECT_ROOT, 'client_secrets.json')
CLIENT_ID = json.load(open(json_url))['web']['client_id']
Use json_url instead client_secrets.json in script

Reffered from stack overflow

Install and configuring postgresql for project
Install Postgres sudo apt-get install postgresql

login to postgres sudo su - postgres

postgres shell psql

create user CREATE USER catalog WITH PASSWORD 'password';

permit user to createdb ALTER USER catalog CREATEDB;

Create a db name catalog with user catalog CREATE DATABASE catalog WITH OWNER catalog;

connect to db \c catalog

revoke all permission to public REVOKE ALL ON SCHEMA public FROM public;

Give schema permission to user catalog GRANT ALL ON SCHEMA public TO catalog;

exit from db and postgres \q and exit

Change the database connection in both db_setup.py and __init__.py as engine = create_engine('postgresql://catalog:password@localhost/catalog')

Now you are ready with your applicatiom

Configure and Enable a New Virtual Host
sudo nano /etc/apache2/sites-available/FlaskApp.conf

In this add the following code

<VirtualHost *:80>
 	ServerName mywebsite.com
 	ServerAdmin admin@mywebsite.com
 	WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
 	<Directory /var/www/FlaskApp/FlaskApp/>
 		Order allow,deny
 		Allow from all
 	</Directory>
 	Alias /static /var/www/FlaskApp/FlaskApp/static
 	<Directory /var/www/FlaskApp/FlaskApp/static/>
 		Order allow,deny
 		Allow from all
 	</Directory>
 	ErrorLog ${APACHE_LOG_DIR}/error.log
 	LogLevel warn
 	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Enable the virtual host sudo a2ensite FlaskApp

Disabling the default apache2 page sudo a2dissite 000-default.conf

Create the .wsgi File
```
cd /var/www/FlaskApp
sudo nano flaskapp.wsgi 
```
Add the following code

#!/usr/bin/python
 import sys
 import logging
 logging.basicConfig(stream=sys.stderr)
 sys.path.insert(0,"/var/www/FlaskApp/")

 from FlaskApp import app as application
 application.secret_key = 'Add your secret key'
save and exit

Deploying flask app with apache2 is referred from Digital ocean

Installing require modules

You can either install all modules on your machine or create a virtual environment for the project and install the modules pip install flask sqlalchemy psycopg2 requests oauth2client

Setting up your Google Oauth2
Login to your developer console and select your project and edit OAuth details as following

Javascript origin http://ip.xip.io

redirect URI

http://ip.xip.io\login

http://ip.xip.io\gconnect

http://ip.xip.io\callback

xip.io is a free DNS which will be the same as using IP address

Final Step

Restart your apache2 server

sudo service apache2 restart