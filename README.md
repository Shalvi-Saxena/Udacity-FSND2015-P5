# Udacity Full Stack Nanodegree Project 5 - Linux Server Configuration

## Project Location

* Server IP: 52.36.219.116
* Port: 2200
* Project Accessible at: http://ec2-52-36-219-116.us-west-2.compute.amazonaws.com/
* SSH access via `ssh -i ~/.ssh/grader.rsa grader@52.36.219.116 -p 2200`


# TODO: check links

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
* Add the following text to the newly created file: `grader ALL=(ALL:ALL) NOPASSWD:ALL`
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

  ![Apache Default Page](https://github.com/larrytooley/Udacity-FSND2015-P5/blob/master/img/apache_default.png)
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

## Install PostgreSQL

* Install PostgreSQL:

  ```shell
  sudo apt-get update
  sudo apt-get install postgresql postgresql-contrib
  ```

* Ensure remote connections are disabled.

  `sudo nano /etc/postgresql/9.3/main/pg_hba.conf`

  * The default configuration disables remote connections by default.  Here is a cleaned up version of the section that controls connections.


  | Type    |Database       |User          |Address                 | Method |
  | ------- | ------------- | ------------ | ---------------------- | ------ |
  |local    |all            |postgres      |                        | peer   |
  |local    |all            |all           |                        | peer   |
  |host     |all            |all           | 127.0.0.1/32           | md5    |
  |host     |all            |all           | ::1/128                | md5    |

  The host IPs point to local addresses by default.

* Create a new role named **catalog** with:
  ```shell
  sudo -u -i postgres
  psql```

  ```sql
  CREATE USER catalog WITH CREATEDB;
  \password catalog
  ```
  Enter the desired password.

* By default PostgreSQL requires a matching system user for any role. For example, it expects a system user **catalog** when attempting to login with:
```
sudo su - catalog
sudo: unable to resolve host ip-10-20-7-202
No passwd entry for user 'catalog'
```

* In order to avoid creating an additional system user with the user name **catalog** we login to with the following and enter the password we provided previously:
```shell
psql -U catalog -d postgres -h 127.0.0.1 -W
```

* Check that **catalog** is the current user:
```sql
SELECT current_user;
```
* Create the catalog database:
```sql
CREATE DATABASE catalog WITH OWNER catalog;
```
* Switch to **catalog** database:
```sql
postgres=> \c catalog
Password for user catalog:
SSL connection (cipher: DHE-RSA-AES256-GCM-SHA384, bits: 256)
You are now connected to database "catalog" as user "catalog".
catalog=>
```

* Resources used for this step.
  * https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04
  * https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps
  * https://www.digitalocean.com/community/tutorials/how-to-use-roles-and-manage-grant-permissions-in-postgresql-on-a-vps--2
  * http://www.postgresql.org/message-id/20040930142124.GA72856@winnie.fuhr.org

## Install Catalog App

* Install git:
```shell
sudo apt-get install -y git
```
* Move to the directory where the app will be installed and clone app:
```shell
cd /var/www/
sudo mkdir catalog
cd catalog
sudo git clone https://github.com/larrytooley/Udacity-FSND2015-P3.git catalog
```
* Create init file and add the following code to test the Apache setup:
```shell
sudo nano __init__.py
```
```python
from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():
    return "Hello, I love Digital Ocean!"
if __name__ == "__main__":
    app.run()
```
* Rename the main application file to **__init__.py**.

* Set up environment and install Flask:
```shell
sudo apt-get install python-pip
sudo pip install virtualenv
cd /var/www/catalog/catalog/
sudo virtualenv venv
source venv/bin/activate
sudo pip install Flask
deactivate
```
* Configure and Enable New Virtual host
  * Create a new configuration file:
  ```shell
  sudo nano /etc/apache2/sites-available/catalog.conf
  ```
  * Add this code to **catalog.conf**:
  ```
  <VirtualHost *:80>
                ServerName http://ec2-52-36-219-116.us-west-2.compute.amazonaws.com
                ServerAdmin larry@larrytooley.com
                WSGIScriptAlias / /var/www/catalog/catalog.wsgi
                <Directory /var/www/catalog/catalog/>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/catalog/catalog/static
                <Directory /var/www/catalog/catalog/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
* Enable the virtual host:
```shell
sudo a2ensite catalog
```
* Create a **.wsgi** file:
```
cd /var/www/catalog
sudo nano catalog.wsgi
```
* Add code to file:
```python
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalog/")
from catalog import app as application
application.secret_key = 'Add your secret key'
```
* I generated application.secret_key locally and substituted it in the file:
```shell
python
```
```python
import os
os.urandom(24)
```
* Restart apache2
```shell
sudo service apache2 restart
```
* Secure .git
  * Create an **.htaccess** file in the **.git** directory:
  ```shell
cd /var/www/catalog/catalog/.git
sudo nano .htaccess
```
  * Add the following code to the file:
  ```shell
Order allow,deny
Deny from all
```

* Resources used for this step:
  * https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
  * http://flask.pocoo.org/docs/0.10/quickstart/#sessions
  * http://stackoverflow.com/questions/6142437/make-git-directory-web-inaccessible

* Install Dependancies
  ```shell
  cd /var/www/catalog/catalog
  source venv/bin/activate
  sudo pip install httplib2 requests oauth2client sqlalchemy psycopg2
  deactivate
  ```

  * Update the database connection to use PostgreSQL by change the refence to SQLite to the following to the **db_model.py** and **__init__.py**:
  ```python
  'postgresql://catalog:<password>@localhost/catalog'
  ```



   http://stackoverflow.com/questions/29134512/insecureplatformwarning-a-true-sslcontext-object-is-not-available-this-prevent

## Remaining Tasks

  * Set up your Catalog App project (from your GitHub repository from earlier in the Nanodegree program) so that it functions correctly when visiting your server’s IP address in a browser.
  * Write up how to fix the sudo unable to resolve host problem.
Your Amazon EC2 Instance's public URL will look something like this: http://ec2-XX-XX-XXX-XXX.us-west-2.compute.amazonaws.com/ where the X's are replaced with your instance's IP address. You can use this url when configuring third party authentication.


Additional Functionality

In addition to the basic functions listed above, this project has several opportunities to go above and beyond what is required. These entail configuring more sophisticated server monitoring and security update processes.
If you choose to implement these features note them in the README file included with your project submission. Mention what features you added and how your evaluator should use or verify the feature.
To Submit


[Project Guide](https://docs.google.com/document/d/1J0gpbuSlcFa2IQScrTIqI6o3dice-9T7v8EDNjJDfUI/pub?embedded=true)
