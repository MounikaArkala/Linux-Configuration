# Linux-Configuration
Project Linux Configuration consits of baseline installation of a Linux server and prepare it to host your web applications. It also has secure the server from a number of attack vectors, install and configure a database server, and deploy one of my existing web applications onto it.
 
## Get Started on Amazon Lightsail
1) First, log in to Lightsail. If you don't already have an Amazon Web Services account, you'll be prompted to create one.
2) Once you're logged in, Lightsail will give you a friendly message with a robot on it, prompting you to create an instance. A Lightsail instance is a Linux server running on a virtual machine inside an Amazon datacenter.
3) Lightsail supports a lot of different instance types. An instance image is a particular software setup, including an operating system and optionally built-in applications.
For this project, you'll want a plain Ubuntu Linux image. There are two settings to make here. First, choose "OS Only" (rather than "Apps + OS"). Second, choose Ubuntu as the operating system.
4) The instance plan controls how powerful of a server you get. It also controls how much money they want to charge you. And as long as you complete the project within a month and shut your instance down, the price will be zero.
5) Every instance needs a unique hostname. You can use any name you like, as long as it doesn't have spaces or unusual characters in it. Your instance's name will be visible to you and to the project reviewer.
6) It may take a few minutes for your instance to start up.
7) Once your instance has started up, you can log into it with SSH from your browser.
The public IP address of the instance is displayed along with its name. In the above picture it's XX.XX.XX.XXX. The DNS name of this instance is ec2-XX-XX-XX-XXX.compute-1.amazonaws.com.

## Secure the server
1. Update all currently installed packages using the command ``` sudo apt-get update ```
2. Install newer versions of the packages  using the command ``` sudo apt-get upgrade ```
3. Open the /etc/ssh/sshd_config file
4. _Change the port number from 22 to 2200 on line 5. 
5. Restart the SSH port using the command ``` sudo service ssh restart ```
6. Set the ufw firewall to block everything coming in using the command ```sudo ufw default deny incoming```
7. Set the ufw firewall to allow everything outgoing using the command ```sudo ufw default allow outgoing```
8. Set the ufw firewall to allow SSH using the command ```sudo ufw allow ssh```
9. Allow all tcp connections for port 2200 using the command ```sudo ufw allow 2200/tcp```
10. Set the ufw firewall to allow a basic HTTP server using the command ```sudo ufw allow www ```
11. Set the ufw firewall to allow NTP using the command ```sudo ufw allow 123/udp```
12. To deny port 22 use the command ```sudo ufw deny 22```
13. To enable the ufw firewall use the command ```sudo ufw enable```
14. To check which ports are open use the command ```sudo ufw status```
15. To login, open up the terminal and run ```ssh -i ~/.ssh/lightrail_key.rsa -p 2200 ubuntu@XX.XX.XX.XX```


## Create a new user 'grader' and create SSH pair for grader
1. To create the user grader use the command ```sudo adduser grader```
2. Enter the passwd when prompted and fill the optional information of the user.
3. To give sudo permissions for grader, use the command ```sudo visudo```
4. search for the line``` root ALL=(ALL:ALL) ALL```, and type ```grader ALL=(ALL:ALL) ALL```
5. save and close the file.
6. generate SSH pair using the command ssh-keygen
7. choose a file name (.ssh/fileName), enter the passphrase twice.
8. Two files are generated  .ssh/fileName and .ssh/fileName.pub
9. We need the contents of fileName.pub for step 13 , so type the command ```cat .ssh/fileName.pub``` and copy the contents.
10. login as grader using the command ```sudo su - grader```.
11. Create a new directory called .ssh using the command ```mkdir .ssh```
12. Use the command ```touch .ssh/authorized_keys```
13. edit .ssh/authorized_keys file and paste the content we got from step 9.
14. run ```chmod 700 .ssh```
15. run ```chmod 644 .ssh/authorized_keys```
16. To force the key based authentication, open /etc/ssh/sshd_config 
17. search for the line PassWord Authentication and change from no to yes if the line says no.
18. now use the command ```sudo servce ssh restart``` to restart
19. Switch to ubuntu user using the command ```exit```
20. Now you can login as a grader using the command ```ssh -i ~/.ssh/grader_key -p 2200 grader@XX.XX.XX.XX```

## Configure the local timezone to UTC
1. To configure the local time use the command ```sudo dpkg-reconfigure tzdata```
2. UTC is under the 'None of the above' category

## Prepare to deploy your project
1. To install Apache use the command ```sudo apt-get install apache2```, verify if the apache is installed or not by using the public IP of Amazon Lightsail as a URL in the browser. A page with title  'Apache2 Ubuntu Default Page' should be loaded. 
2. We need to install mod_wsgi _package to serve flask applications and python-dev wich consists of header files required for building python extensions. For this, use the command : ```sudo apt-get install libapache2-mod-wsgi python-dev```
3. Enable  mod_wsgi _using the command  ```sudo a2enmod wsgi```
4. Install PostgreSQL using the command ```sudo apt-get install postgresql```
5. By default PostgreSQL creates a Linux user with the name postgres during installation; switch to this user using the command ```sudo su - postgres```.
6. Connect to psql using the command ```psql```
7. Create the catalog user using the command ```CREATE ROLE catalog WITH LOGIN;```
8. Give the catalog user the ability to create databases using the command ```ALTER ROLE catalog CREATEDB;```
8. Give the catalog user the ability to create databases using the command ```ALTER ROLE catalog CREATEDB;```
9. Give the catalog user a password using the command ```\password catalog```
10. To verify if the catalog user was created or not use the command ```\du; ```
11. Exit psql using the command ```\q```
12. Switch back to the ubuntu user by typing ```exit```
13. Create a new Linux user called catalog using the command ```sudo adduser catalog;``` Fill out all required information for catalog
14. To give the catalog user sudo permissions use the command  ```sudo visudo```
15. Search for a line that looks like this: ```root ALL=(ALL:ALL) ALL``` and add the following line below this one: ```catalog ALL=(ALL:ALL) ALL```
16. Save and close the visudo file
17. Switch to  user 'catalog' using the command ```sudo su - catalog```.
18. Create a database called catalog using the command ```createdb catalog```
19. Connect to psql using the command ```psql``` and type ```\l``` to see the new database 'catalog' is listed or not. 
20. Exit psql using the command ```\q```
21. Switch back to the ubuntu user by typing ```exit```
22. Install git using the command ```sudo apt-get install git```

## Deploy the Item Catalog project.

1. Create a directory called 'itemCatalog' in the /var/www/ directory

2. cd to the 'nuevoMexico' directory, and clone the itemCatalog project using the command: ```sudo git clone https://github.com/MounikaArkala/ItemCatalog.git itemCatalog```

3. Change the ownership of the 'itemCatalog' directory to ubuntu using the command ```sudo chown -R ubuntu:ubuntu itemCatalog/```

4. cd to the /var/www/itemCatalog/itemCatalog directory

5. Change the name of the project.py file to __init__.py by running ```mv project.py __init__.py```

6. In __init__.py, find line 508:

```app.run(host='0.0.0.0', port=5656)```

Change this line to:

```app.run()```

7. Log in to Google API Console and edit the Client ID for Web application
8. Add http://XX.XX.XX.XX and http://ec2-XX-XX-XX-XX.compute-1.amazonaws.com as authorized JavaScript origins
9. Add http://ec2-XX-XX-XX-XX.compute-1.amazonaws.com/login, http://ec2-XX-XX-XX-XX.compute-1.amazonaws.com/gconnect, and http://ec2-XX-XX-XX-XX.compute-1.amazonaws.com/oauth2callback as authorized redirect URIs

10. Download the JSON file and save it as client_secrets.json in the /var/www/itemCatalog/itemCatalog/ directory

11. Add the complete file path for the client_secrets.json file in lines 33 and 63 in the __init__.py file; change it from 'client_secrets.json' to '/var/www/itemCatalog/itemCatalog/client_secrets.json'_
12. Install pip  with the using the command ```sudo apt-get install python-pip```
13. Install virtualenv using the command ``` sudo apt-get install python-virtualenv```
14. cd to the /var/www/itemCatalog/itemCatalog/ directory; choose a name for a temporary environment let's say 'venv' for now. To create this environment use the command ```virtualenv venv```.
15. To activate the new environment, venv use the command ```. venv/bin/activate```
16. While the virtual environment active, install the following dependenies

```pip install httplib2```

```pip install requests```

```pip install --upgrade oauth2client```

```pip install sqlalchemy```

```pip install flask```

```sudo apt-get install libpq-dev ```

```pip install psycopg2```

17. To verify everything was installed correctly, run python __init__.py; the followingshould be returned:

* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)

18 To Deactivate the virtual environment use the command  ```deactivate```

19. Create a file in /etc/apache2/sites-available/ called itemCatalog.conf

Add the following into the file:

``` <VirtualHost *:80>
		ServerName XX.XX.XX.XX
		ServerAdmin xxxx@domain.com
		WSGIScriptAlias / /var/www/itemCatalog/itemCatalog.wsgi
		<Directory /var/www/itemCatalog/itemCatalog/>
			Order allow,deny
			Allow from all
			Options -Indexes```
		</Directory>
		Alias /static /var/www/itemCatalog/itemCatalog/static
		<Directory /var/www/itemCatalog/itemCatalog/static/>
			Order allow,deny
			Allow from all
			Options -Indexes
		</Directory>
		ErrorLog ${APACHE_LOG_DIR}/error.log
		LogLevel warn
		CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>


20.  To enable the virtual host use the command ```sudo a2ensite itemCatalog ```

21. To activate the new configuration, you need to run: ```sudo service apache2 reload```
22. Apache serves Flask applications by using a .wsgi file; So create a file called itemCatalog.wsgi in /var/www/itemCatalog

Add the following to the file:
```
activate_this = '/var/www/itemCatalog/itemCatalog/venv/bin/activate_this.py'
execfile(activate_this, dict(__file__=activate_this))

#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/itemCatalog/")

from nuevoMexico import app as application
application.secret_key = 'xxxx'
```
_23. Resart Apache using the command ```sudo service apache2 restart```
24. Replace line 38 in __init__.py, line 85 in database_setup.py, and line 7 in lotsofmenus.py with the following:

```engine = create_engine('postgresql://catalog:INSERT_PASSWORD_FOR_DATABASE_HERE@localhost/catalog')```
_25. To disable the default Apache site  use the command ```sudo a2dissite 000-default.conf```
26. Run ```sudo service apache2 reload``` to reload apache
27. Change the ownership of the project directories and files to the www-data user (this is done because Apache runs as the www-data user); while in the /var/www directory, run:

```sudo chown -R www-data:www-data itemCatalog/```
28. cd to /var/www/itemCatalog/itemCatalog/ directory, activate the virtualenv by running ```. venv/bin/activate```

29. Run``` python lotsofmenus.py```
30. Deactivate the virtualenv using the command ```deactivate```
31.  Resart Apache using the command ```sudo service apache2 restart```

Open up a browser and check to make sure the app is working by going to ```http://XX.XX.XX.XX``` or ```http://ec2-XX-XX-XX-XX.compute-1.amazonaws.com```



