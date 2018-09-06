## Useful shell scripts for Laravel/Homestead


### 1. Installation

~~~
git clone https://pildog@github.com/pildog/homestead-scripts.git ~/homestead-scripts
~~~

Create a mapping in `Homestead.yaml` file

~~~
folders:
	
	# change to your path
	- map: ~/homestead-scripts
      to: /home/vagrant/bin
~~~

---

### 2. Before you use the scripts

#### 2.1. Creating a backup folder

Create backup folder mapping in `Homestead.yaml` file

~~~
folders:
	
	# change to your path
	- map: ~/tmp/backup
      to: /home/vagrant/backup
~~~

Once you edit your `Homestead.yaml` file. Restart your vagrant host by using following commands

~~~
vagrant halt
vagrant up
~~~

#### 2.2. Configuring settings

Once your vagrant has finished loading, type `vagrant ssh` to connect to your host.

##### 2.2.1. Change permissions

~~~
chmod 755 ~/bin/db/*
chmod 755 ~/bin/mailcatcher/*
~~~

##### 2.2.2. Creating config file for the scripts

Run following command to copy from a sample.

~~~
cp ~/bin/config.sample ~/bin/config
~~~


##### 2.2.3. Creating database credential file

Laravel Homestead default database username and password is 'homestead' and 'secret'. Use the following command to create a mysql credential file.

~~~
~/bin/db/create_cred homestead secret
~~~


##### 2.2.4. Modifying settings

Change path if nesseccary.

`~/bin/config`

~~~
#!/usr/bin/env bash

# path to the credential file for mysql
CREDENTIAL_PATH="/home/vagrant/.my.cnf"

# Where your DB backup will be saved
BACKUP_PATH="/home/vagrant/backup"

# mailcatcher smtp port
SMTP_PORT="9999"

# mailcatcher web port
WEB_PORT="1080"

# Defines the folder where the mailcatcher startup script will be saved
SCRIPT_PATH="/home/vagrant/mailcatcher"
~~~

---
### 3. Using scripts

#### 3.1. Database

#### 3.1.1. Available scripts

`~/bin/db/backup_all` Backup all databases

`~/bin/db/restore_all` Restore from backup archive

`~/bin/db/import` Run SQL script to target database

#### 3.1.2 Backup all

Running the script will create a folder and back up all databases inside. 

~~~
~/bin/db/backup_all
~~~

#### 3.1.3 Restore all

Passing a folder name as a parameter will restore all databases inside of the folder.

~~~
~/bin/db/backup_all [YYYY-MM-DD-HH-MM-SS]
~~~


#### 3.1.4 Import

This script will look for the given SQL script file from your backup folder. If the database doesn't exist, it will create one.

~~~
~/bin/db/import [SQL script filename] [database name]
~~~

#### 3.2. Mailcatcher Setup

#### 3.2.1. Available scripts

`~/bin/mailcatcher/setup` Mailcatcher setup scripts

#### 3.2.2. Mailcatcher

Mailcatcher is a fake SMTP program that catch all outgoing e-mails instead of actually delivering to recipients. After thed script finishes installation, it will give you URL to access via Web UI.

~~~
$ bin/mailcatcher/setup
mailcatcher setup completed. See your emails at http://192.168.10.10:1080
~~~

You can type `http://192.168.10.10:1080` on your web browser to access Web GUI.

#### 3.2.2.1 Network interface option

If you want to point a different network interface. put the name of the interface after as an argument.

~~~
bin/mailcatcher/setup eth2
~~~ 