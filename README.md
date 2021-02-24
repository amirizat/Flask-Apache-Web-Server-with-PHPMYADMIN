# Flask-Apache-Web-Server-with-PHPMYADMIN
Install and configure Flask PHPMYADMIN with APACHE

First install apache with mysql
-sudo apt-get install apache2 mysql-client mysql-server

Setup mysql
-sudo mysql_secure_installation
-sudo mysql -u root -p

Installing mysql python libary
-sudo apt-get install python-mysqldb

Creating a Database user 
-sudo mysql -u root -p

-GRANT ALL PRIVILEGES ON mydb.* TO 'username'@'localhost' IDENTIFIED BY 'password';
#username and password cant be anything

Installing PHPMYADMIN
-sudo apt-get install phpmyadmin

Configuring Apache server 
-sudo nano /etc/apache2/apache2.conf

#include this line
-Include /etc/phpmyadmin/apache.conf

-sudo /etc/init.d/apache2 restart

Open this on a web browser
-localhost/phpmyadmin​

Install WSGI 
-sudo apt-get install libapache2-mod-wsgi

Enabled WSGI 
-sudo a2enmod wsgi

Set up Flask environment
Go to the directory
-cd /var/www/

Make Flask environment directory
-mkdir FlaskApp

Move into directory
-cd FlaskApp

Make another directory
-mkdir FlaskApp

#/to/flask/directory/FlaskApp/FlaskApp
Move into directory
-cd FlaskApp/

Make two directories, static
mkdir static
mkdir templates

Create first Flask App:
sudo nano __init__.py


#! /bin/usr/python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def homepage():
    return "Hello World"


if __name__ == "__main__":
    app.run()

Press control+x to save it, yes, enter

Proceed installing flask. Update and upgrade first
-apt-get update
-apt-get upgrade

Install pip first before installing flask
-apt-get install python-pip

Install virtual environment
-pip install virtualenv

Now to set up the virtualenv directory
-sudo virtualenv venv

Activate the virtual environment
-source venv/bin/activate

Now install Flask within virtual environment
-pip install Flask

Testing pyhton code
-python __init__.py #for pyhton2
-python3 __init__.py #for pyhton3

Hit control+c to out form application

Stop virtual environment
-deactivate

Setup Flask configuration file
-nano /etc/apache2/sites-available/FlaskApp.conf

<VirtualHost *:80>
                ServerName yourdomain.com(ipaddress)
                ServerAdmin youremail@email.com
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


Run
-sudo a2ensite FlaskApp
-service apache2 reload

Configure WSGI file
-cd /var/www/FlaskApp

nano flaskapp.wsgi
-copy paste this code


#!/usr/bin/python

'''add this code if using virtual environment
activate_this = '/var/www/FlaskApp/FlaskApp/venv/bin/activate_this.py'
execfile(activate_this, dict(__file__=activate_this))
'''

import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/")

from FlaskApp import app as application
application.secret_key = "abc1234 (can be anything)"



Save and exit

Restart apache
-service apache2 restart


Goto web browser enter ip address
