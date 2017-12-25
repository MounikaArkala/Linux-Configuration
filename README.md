# Linux-Configuration
Project Linux Configuration consits of baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.
 
## Get Started on Amazon Lightsail
1) First, log in to Lightsail. If you don't already have an Amazon Web Services account, you'll be prompted to create one.
2) Once you're logged in, Lightsail will give you a friendly message with a robot on it, prompting you to create an instance. A Lightsail instance is a Linux server running on a virtual machine inside an Amazon datacenter.
3) Lightsail supports a lot of different instance types. An instance image is a particular software setup, including an operating system and optionally built-in applications.
For this project, you'll want a plain Ubuntu Linux image. There are two settings to make here. First, choose "OS Only" (rather than "Apps + OS"). Second, choose Ubuntu as the operating system.
4) The instance plan controls how powerful of a server you get. It also controls how much money they want to charge you. For this project, the lowest tier of instance is just fine. And as long as you complete the project within a month and shut your instance down, the price will be zero.
5) Every instance needs a unique hostname. You can use any name you like, as long as it doesn't have spaces or unusual characters in it. Your instance's name will be visible to you and to the project reviewer.
6) It may take a few minutes for your instance to start up.
7) Once your instance has started up, you can log into it with SSH from your browser.
The public IP address of the instance is displayed along with its name. In the above picture it's XX.XX.XX.XXX. The DNS name of this instance is ec2-XX-XX-XX-XXX.compute-1.amazonaws.com.

## Secure the server
1. Update all currently installed packages using the command sudo apt-get update 
2. Install newer versions of the packages  using the command sudo apt-get upgrade
3. Open the /etc/ssh/sshd_config file
4. _Change the port number from 22 to 2200 on line 5. 
5. Restart the SSH port using the command sudo service ssh restart.
6. Set the ufw firewall to block everything coming in using the command sudo ufw default deny incoming
7. Set the ufw firewall to allow everything outgoing using the command sudo ufw default allow outgoing
8. Set the ufw firewall to allow SSH using the command sudo ufw allow ssh
9. Allow all tcp connections for port 2200 using the command sudo ufw allow 2200/tcp
10. Set the ufw firewall to allow a basic HTTP server using the command sudo ufw allow www 
11. Set the ufw firewall to allow NTP using the command sudo ufw allow 123/udp
12. To deny port 22 using the command sudo ufw deny 22
13. To enable the ufw firewall use the command sudo ufw enable
14. To check which ports are open use the command sudo ufw status
15. To login open up the terminal and run ssh -i ~/.ssh/lightrail_key.rsa -p 2200 ubuntu@XX.XX.XX.XX


## Create a new user 'grader' and create SSH pair for grader
1. To create the user grader use the command sudo adduser grader
2. Enter the passwd when prompted and fill the optional information of the user.
3. To give sudo permissions for grader, use the command sudo visudo
4. search for the line root ALL=(ALL:ALL) ALL, and type grader ALL=(ALL:ALL) ALL
5. save and close the file.
6. generate SSH pair using the command ssh-keygen
7. choose a file name (.ssh/fileName), enter the passphrase twice.
8. Two files are generated  .ssh/fileName and .ssh/fileName.pub
9. We need the contents of fileName.pub for step 13 , so type the command cat .ssh/fileName.pub
10. login as grader using the command sudo su - grader.
11. Create a new directory called .ssh: mkdir .ssh
12. Use the command touch .ssh/authorized_keys
13. edit .ssh/authorized_keys file and paste the content we got from step 9.
14. run chmod 700 .ssh
15. run chmod 644 .ssh/authorized_keys
16. To force the kry based authentication, open /etc/ssh/sshd_config 
17. search for the line PassWord Authentication and change from no to yes if the line says no.
18. now use the command sudo servce ssh restart to restart
19. Switch to ubuntu user using the command exit
20. Now you can login as a grader using the command ssh -i ~/.ssh/grader_key -p 2200 grader@XX.XX.XX.XX

## Configure the local timezone to UTC
1. To configure the local time use the command sudo dpkg-reconfigure tzdata
2. UTC is under the 'None of the above' category

## Prepare to deploy your project
1. To install Apache use the command sudo apt-get install apache2, verify if the apache is installed or not by using the public IP of Amazon Lightsail as a URL in the browser. A page with title  'Apache2 Ubuntu Default Page' should be loaded. 
2. We need to install mod_wsgi _package to serve flask applications and python-dev wich consists of header files required for building python extensions. For this, use the command : sudo apt-get install libapache2-mod-wsgi python-dev
3. Enable  mod_wsgi _using the command  sudo a2enmod wsgi
4. Install PostgreSQL using the command sudo apt-get install postgresql
5. By default PostgreSQL creates a Linux user with the name postgres during installation; switch to this user using the command sudo su - postgres.
6. Connect to psql using the command psql
7. Create the catalog user using the command CREATE ROLE catalog WITH LOGIN;
8. Give the catalog user the ability to create databases using the command ALTER ROLE catalog CREATEDB;
9. Give the catalog user a password using the command \password catalog
10. To verify if the catalog user was created or not use the command \du; 
11. Exit psql using the command \q
12. Switch back to the ubuntu user by typing exit
13. Create a new Linux user called catalog using the command sudo adduser catalog; Fill out all required information for catalog
14. To give the catalog user sudo permissions use the command  sudo visudo
15. Search for a line that looks like this: root ALL=(ALL:ALL) ALL and add the following line below this one: catalog ALL=(ALL:ALL) ALL
16. Save and close the visudo file
17. Switch to  user 'catalog' using the command sudo su - catalog.
18. Create a database called catalog using the command createdb catalog
19. Connect to psql using the command psql and type \l to see the new database 'catalog' is listed or not. 
20. Exit psql using the command \q
21. Switch back to the ubuntu user by typing exit
22. Install git using the command sudo apt-get install git\

## Deploy the Item Catalog project.




