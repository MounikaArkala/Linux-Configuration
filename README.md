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
