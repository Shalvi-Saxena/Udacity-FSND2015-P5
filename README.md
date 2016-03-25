# Udacity Full Stack Nanodegree Project 5 - Linux Server Configuration

## Project Location
* Server IP: 52.36.219.116
* Port: 2200
* Project Accesible at: http://ec2-52-36-219-116.us-west-2.compute.amazonaws.com/
* SSH access via `ssh -i ~/.ssh/grader.rsa grader@52.11.69.16 -p 2200`

## Table of Contents
* [Create Grader Account](#create-grader-account)
* [Give Grader SUDO](#give-grader-sudo)
* [Create SSH Keys](#create-ssh-keys)
* [Update Packages](#update-packages)
* [Configure Timezone](#configure-timezone)
* [Change SSH Port](#change-ssh-port)
* [Configure Firewall](#configure-firewall)
* [Install Apache](#install-apache)
* [Serve A Python mod_wsgi Application](#serve-a-python-mod_wsgi-application)
* [Install PostgreSQL](#install0-postgresql)
* [Install Git](#install-git)
* [Install App](#install-app)
* [Additional Steps](#additional-steps)


## Create grader Account
* Created **grader** account with the following command: `sudo adduser grader`
* Resources used for this step.
  * https://www.udacity.com/course/viewer#!/c-ud299-nd/l-4331066009/m-4801089468


## Give **grader** SUDO
* Create the file `grader` in /etc/sudoer.d/ with `touch /etc/sudoers.d/grader`
* Add the following text to the newly created file: `grader ALL=(ALL:ALL) ALL`
* Resources used for this step.
  * https://www.udacity.com/course/viewer#!/c-ud299-nd/l-4331066009/m-4801089471


## Create SSH Keys
* Create SSH key with **ssh-keygen**
* Create an `.ssh` direcory in `/home/grader/` on the server with `mkdir .ssh`.
* CD into the the directory just created with `cd ~/.ssh/`.
* Create an `authorized_keys` file in the `.ssh` dirctory with `touch authorized_keys`.
* Paste the public key into `/home/grader/.ssh/authorized_keys`
* Set directory permissions
  * Using `chmod` set `~/.ssh` to 700 with `chmod 700 /home/grader/.ssh/`.
  * Again, using `chmod` set the `authorized_keys` file to 644 with `chmod 644 /home/grader/.ssh/authorized_keys`.
* Check owner and group of `~/.ssh` and `~/.ssh/authorized_keys`.
* If the owner and group are not **grader**, set them to **grader** with
* Check to ensure you can log into the **grader** account with `ssh -i ~/.ssh/grader.rsa  grader@52.36.219.116`.
  * Recheck you followed the steps above in the event of an issue or Google the error message.  This how I figured out that password login was disabled on my instance already.
* Resources used for this step.
  * https://www.udacity.com/course/viewer#!/c-ud299-nd/l-4331066009/m-4801089477
  * https://www.udacity.com/course/viewer#!/c-ud299-nd/l-4331066009/m-4801089481


##  Update Packages
* Use the following commands to update the packages on the server.
  * `sudo apt-get update`
  * `sudo apt-get upgrade`. Type "Y" when asked if you would like to proceed.
*  Resources used in this step.
  * https://help.ubuntu.com/community/AptGet/Howto


## Configure Timezone
* Check the current timezone with `date`.
* If you do not set UTC in the output change the timezone with `dpkg-reconfigure tzdata`.
* Resources used for this step.
  * https://help.ubuntu.com/community/UbuntuTime


## Change SSH Port
* Use `nano` to edit the SSH config file with `sudo /etc/ssh/sshd_config`.
* Change the default port from 22 to 2200 by changing the following

```
# What ports, IPs and protocols we listen for
Port 22
```

 to

 ```
 # What ports, IPs and protocols we listen for
 Port 2200
 ```

* Check to see that password login is disabled.
  * You should see the following in the file. If set to "yes" change it to "no" and save the file.
```
# Change to no to disable tunnelled clear text passwords
PasswordAuthentication no
```
* Resart ssh with `sudo service ssh restart`


## Configure Firewall
* Check the status of the firewall with `sudo ufw status`.
* Ensure that by default inbound connections are denied with `sudo ufw default deny incoming`.
* Ensure the all outbound connections are allowed with `sudo ufw default allow outgoing`.
* Open ports for SSH, HTTP, and NTP with the following commands.
  * `sudo ufw allow 2200/tcp`
  * `sudo ufw allow www`
  * `sudo ufw allow ntp`
* Activate the firewall with `sudo ufw enable`.
* Resources used for this step.
  * https://www.udacity.com/course/viewer#!/c-ud299-nd/l-4331066009/m-4801089498


## Install your application

### Install Apache
* Check to see if Apache is installed with `apache2 -v`
* If Apache is installed you will see something like this:

  `Server version: Apache/2.4.7 (Ubuntu)`

  `Server built:   Jan 14 2016 17:45:23`
* If you do not have Apache installed you will see a message like this:

  `The program 'apache2' is currently not installed. To run 'apache2' please ask your administrator to install the package 'apache2-bin'`
* To install Apache use the following commands:
  `sudo apt-get update`
  `sudo apt-get install apache2`
* If you have installed Apache correctly you should see this page at the public IP address.

  ![Apache Default Page](https://github.com/larrytooley/Udacity-FSND2015-P5/img/apache-default.png)
* Resources used for this step.
  * https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04

### Install mod_wsgi
* Install libapache2-mod-wsgi with this command:

  `sudo apt-get install libapache2-mod-wsgi python-dev`
* Enable libapache2-mod-wsgi:

  `sudo a2enmod wsgi`
* Resources used for this step.
  * https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
  * https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-apache-and-mod_wsgi-on-ubuntu-14-04

# Remaining Tasks
* Install and configure PostgreSQL:
  * Do not allow remote connections
  * Create a new user named catalog that has limited permissions to your catalog application database
  * Install git, clone and set up your Catalog App project (from your GitHub repository from earlier in the Nanodegree program) so that it functions correctly when visiting your server’s IP address in a browser. Remember to set this up appropriately so that your .git directory is not publicly accessible via a browser!
Your Amazon EC2 Instance's public URL will look something like this: http://ec2-XX-XX-XXX-XXX.us-west-2.compute.amazonaws.com/ where the X's are replaced with your instance's IP address. You can use this url when configuring third party authentication.


Additional Functionality

In addition to the basic functions listed above, this project has several opportunities to go above and beyond what is required. These entail configuring more sophisticated server monitoring and security update processes.
If you choose to implement these features note them in the README file included with your project submission. Mention what features you added and how your evaluator should use or verify the feature.
To Submit

Your README.md file should include all of the following:
The IP address and SSH port so your server can be accessed by the reviewer.
The complete URL to your hosted web application.
A summary of software you installed and configuration changes made.
Hint: refer to the .bash_history files on the server!
A list of any third-party resources you made use of to complete this project.
Open your ~/.ssh/udacity_key.rsa file in a text editor and copy the contents of that file.
During the submission process, paste the contents of the udacity_key.rsa file into the "Notes to Reviewer" field.
When you're ready to submit your project, click here and follow the instructions.
If you run into any trouble, send us an e-mail at fullstack-project@udacity.com, and we will be more than happy to help you.
Additional Resources

Students have compiled these lists of useful resources, feel free to use these lists and to contribute your own findings!
Markedly underwhelming and potentially wrong resource list for P5 (This is a good list, despite the title!)
Project 5 Resources
P5 How I got through it


https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04
https://www.digitalocean.com/community/tutorials/how-to-connect-to-your-droplet-with-ssh
https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04
https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-ubuntu-14-04-servers

[Project Guide](https://docs.google.com/document/d/1J0gpbuSlcFa2IQScrTIqI6o3dice-9T7v8EDNjJDfUI/pub?embedded=true)
