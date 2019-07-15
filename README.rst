========
Segfault
========

Segfault is a python based Q&A forum like StackOverflow.
It is based on code of Askbot project which is available at http://askbot.com


Install Instructions
=====================

The following instructions should help you setup 'segfault' on a linux machine (As done on Ubuntu 14.04.6)

Prerequisites:
==============

python-dev
----------
   sudo apt-get install python-dev

MySQL [if you're not using integrated sqlite db]
-----
   It can be installed using {apt-get install } method.
   libmysqlclient-dev will be required, in case it is not installed before.
   sudo apt-get install libmysqlclient-dev

pip
---
   sudo apt-get install python-pip

virtualenv
----------
   sudo pip install virtualenv

python-MySQLdb connector
------------------------
   sudo apt-get install python-mysqldb
   pip install mysql-python

setuptools
----------
   pip install setuptools


Optionally(for developers)
--------------------------
   Install eclipse and 'pydev' plugin on eclipse



Install Steps:
==============

Setup virtual environment
-------------------------
Create a virtual environment where all libraries will be installed to:
   
   virtualenv env

Activate the newly created virtualenv box
   
   source env/bin/activate


Create and Setup Database (MySQL)
---------------------------------
This section assumes that MySQL is installed and is up and running.

Log in to mysql:

   mysql -u username -p

Then type these two commands (note that fake dbname=segfault, dbuser, and dbpassword are used in this example):

   create database segfault DEFAULT CHARACTER SET UTF8 COLLATE utf8_general_ci;
   grant all privileges on segfault.* to dbuser@localhost identified by 'dbpassword';

Again, please remember to create real usernname, database name and password and write them down. These credentials will go into the file settings.py - the main configuration file of the Django application.

 
Clone this git repository 
-------------------------
   cd ~/git/
   git clone https://github.com/chhotaladka/segfault.git

Install segfault
----------------
Go to the installation folder where the repositories were cloned

   cd ~/git/segfault
   python setup.py install

**Note**: If the upstream has been updated and you want to update your currently running setup.
Git pull the upstream and run `python setup.py install`

Create and configure the site files
-----------------------------------
Create deployment directory

   mkdir ~/segfaultsite

When installing for the first time, you will need to initialize the project setup files by typing:

   segfault-setup

and answering the questions. The segfault-setup script will ask you where to deploy Segfault. If you are in the directory where the Segfault project resides, you can answer . (. refers to the current directory). There may be an error message; ignore it.

After that - run command collectstatic - in order to place all the static files (.css and .js) into one directory:

   python manage.py collectstatic

When you install first time and any time you upgrade the software, run these two commands:

   python manage.py syncdb
   python manage.py migrate

Update settings.py
------------------
Edit ~/segfaultsite/settings.py :

   MEDIA_ROOT = os.path.join(os.path.dirname(__file__), 'media')
   MEDIA_URL = '/media/'
   STATIC_URL = '/static/'#this must be different from MEDIA_URL
   
Now, try running the test server :
----------------------------------
   cd ~/segfaultsite
   python manage.py runserver

From the browser, visit: 127.0.0.1:8000
If everthing went well, it should show the welcome page.

Installation under Apache/mod_wsgi
----------------------------------
Install mod_wsgi <Web Server Gateway Interface for Python>

   sudo apt-get install libapache2-mod-wsgi

Configure webserver
Copy/Create segfault.conf file in /etc/apache2/conf-available/segfault.conf

Change USER and GROUP permission of site

   ps aux | grep apache
   groups [USERNAME]
default is => www-data : www-data

   sudo chown -R www-data:www-data ~/segfaultsite


