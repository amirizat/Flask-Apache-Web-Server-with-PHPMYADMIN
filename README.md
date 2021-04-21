
Install and configure Flask phpmyadmin and Apache

First install apache with mysql
<br>
-sudo apt-get install apache2 mysql-client mysql-server

Setup mysql<br>  
 -sudo mysql_secure_installation<br>
-sudo mysql -u root -p

Installing mysql python libary <br>
-sudo apt-get install python-mysqldb

Creating a Database user <br>
-sudo mysql -u root -p

-GRANT ALL PRIVILEGES ON mydb.* TO 'username'@'localhost' IDENTIFIED BY 'password';<br>
#username and password cant be anything

Installing PHPMYADMIN<br>
-sudo apt-get install phpmyadmin

Configuring Apache server <br>
-sudo nano /etc/apache2/apache2.conf

#include this line<br>
-Include /etc/phpmyadmin/apache.conf

-sudo /etc/init.d/apache2 restart

Open this on a web browser<br>
-localhost/phpmyadmin​

Install WSGI <br>
-sudo apt-get install libapache2-mod-wsgi (for python2) <br>
-sudo apt-get install libapache2-mod-wsgi-py3 (for python3)

Enabled WSGI <br>
-sudo a2enmod wsgi

Set up Flask environment<br>
Go to the directory<br>
-cd /var/www/

Make Flask environment directory<br>
-mkdir FlaskApp

Move into directory<br>
-cd FlaskApp

Make another directory<br>
-mkdir FlaskApp

#/to/flask/directory/FlaskApp/FlaskApp<br>
Move into directory<br>
-cd FlaskApp/

Make two directories, static<br>
mkdir static
mkdir templates

Create first Flask App:<br>
sudo nano __init__.py


#! /bin/usr/python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def homepage():
    return "Hello World"


if __name__ == "__main__":
    app.run()

Press control+x to save it, yes, enter<br>

Proceed installing flask. Update and upgrade first<br>
-apt-get update
-apt-get upgrade

Install pip first before installing flask<br>
-apt-get install python-pip

Install virtual environment<br>
-pip install virtualenv

Now to set up the virtualenv directory<br>
-sudo virtualenv venv

Activate the virtual environment<br>
-source venv/bin/activate

Now install Flask within virtual environment<br>
-pip install Flask

Testing pyhton code<br>
-python __init__.py #for pyhton2<br>
-python3 __init__.py #for pyhton3

Hit control+c to out form application<br>

Stop virtual environment<br>
-deactivate

Setup Flask configuration file<br>
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


Run<br>
-sudo a2ensite FlaskApp<br>
-service apache2 reload

Configure WSGI file<br>
-cd /var/www/FlaskApp

nano flaskapp.wsgi<br>
-copy paste this code


#!/usr/bin/python

'''add this code if using virtual environment (for python2) <br>
activate_this = '/var/www/FlaskApp/FlaskApp/venv/bin/activate_this.py'<br>
execfile(activate_this, dict(__file__=activate_this))<br>
'''<br>

'''add this code if using virtual environment (for python3) <br>
activate_this = '/var/www/FlaskApp/FlaskApp/venv/bin/activate_this.py'<br>
with open(activate_this) as file_:<br>
    exec(file_.read(), dict(__file__=activate_this))<br>
'''<br>

import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/")

from FlaskApp import app as application
application.secret_key = "abc1234 (can be anything)"



Save and exit<br>

Restart apache<br>
-service apache2 restart<br>


Goto web browser enter ip address
