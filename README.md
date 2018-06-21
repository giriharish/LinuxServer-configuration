# LinuxServer-configuration
IP address: 13.126.182.198

SSH port: 2200

URL: http://13.126.182.198.xip.io/
About
This is the sixth project for the Udacity Full Stack Nanodegree. This project involves taking a baseline installation of Linux on a virtual machine and preparing it to host web applications. This includes installing updates, securing the server from attacks, and installing / configuring web and database servers.

Set up remote machine
Add a new user called grader: adduser grader
Give the new user sudo privilege: usermod -aG sudo grader
Update package index: apt-get update
Upgrade the installed packages: apt-get upgrade

Setup SSH keys for user grader
In the local machine: ssh-keygen 
SCP public key to the remote machine: scp -p 22 ~/.ssh/id_rsa.pub grader@<ip-address>:~/.ssh
On server machine
 cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
 chmod 700 .ssh
 chmod 600 .ssh/authorized_keys
 rm .ssh/id_rsa.pub

 Disable root login and other security settings
Open ssh config file: sudo nano /etc/ssh/sshd_config
Chage ssh port to 2200
Change PasswordAuthentication to no, changed PermitRootLogin to no and PrintLastLog to no

Change timezone to UTC
Open the timezone selection dialog: sudo dpkg-reconfigure tzdata
Then chose 'None of the above', then UTC.
Setup the ntpd for regular time sync: sudo apt-get install ntp
Choose closer NTP time servers and put in /etc/ntp.conf

Install and configure Apache
apache web server: sudo apt-get install apache2
Install mod_wsgi for serving Python apps from Apache: sudo apt-get install python-setuptools libapache2-mod-wsgi
Restart apache: sudo service apache2 restart

Set up remote machine for flask application
Install additional packages to enable Apache to serve Flask applications: sudo apt-get install libapache2-mod-wsgi python-dev
Create directory for the flask app: cd /var/www
Create directory catalog
Clone the movie catalog from git inside this directory and renamed the top subdirectory as catalog
Install python-pip: sudo apt-get install python-pip
Install virtualenv: sudo pip install virtualenv
Set virtual environment to name 'venv': sudo virtualenv venv
Create a virtual host config file : sudo nano /etc/apache2/sites-available/catalog.conf
Edit the file as per project settings
Enabled the host : sudo a2ensite catalog
Create the .wsgi file catalog.wsgi as per project settings.
Restarted apache: sudo service apache2 restart

Set up the virtual environment for testing project
Activate virtual environment: source venv/bin/activate
Install required packages like httplib2, requests, flask-seasurf, oauth2.client, sqlalchemy and python-psycopg2 inside the virtual environment

##Configure PostgreSQL

Install PostgreSQL: sudo apt-get install postgresql postgresql-contrib
Edit database_setup.py to and edit the create_engine line:
python engine =create_engine('postgresql://catalog:<pwd>@localhost/catalog')
Change the same line in project.py file
Create catalog user for postgresql: sudo adduser catalog
Change to default user postgres:sudo su - postgres
Connect to the system and create user with LOGIN role and set a password: `CREATE USER catalog WITH PASSWORD '';
Allow the user to create database tables: ALTER USER catalog CREATEDB;
Create database: CREATE DATABASE catalog WITH OWNER catalog;
Revoke all rights and grant only access to the catalog role
Create database: python database_setup.py

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