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
  * `sudo apt-get upgrade`



## Login with **grader**
* Use the command `ssh -i ~/.ssh/grader.rsa grader@52.11.69.16 -p 2200`


## Change the SSH Port from 22 to 2200
* Disable password Login
  * `/etc/ssh/sshd_conf`
  * Resart ssh with sudo service ssh restart

## Configure the local timezone to UTC.

## Configure the firewall
Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)

Don't activate firewall until it is configured less ye lock yourself out.

https://www.udacity.com/course/viewer#!/c-ud299-nd/l-4331066009/m-4801089498


## Install your application

### Install and configure Apache to serve a Python mod_wsgi application
### Install and configure PostgreSQL:
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
