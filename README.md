# Python-SQL-password-manager
This is a simple password manager made with python and MariaDB with help from teja156. Project is essentially the same with some different functions added. I used teja156's video to learn more about password managers and start a project for experience, so much if not most of the project is the same is his. Check out the original project here: https://github.com/teja156/python-password-manager/tree/main

# teja156's explanation of the project
A simple local password manager written in Python and MariaDB. Uses [pbkdf2](https://en.wikipedia.org/wiki/PBKDF2) to derive a 256 bit key from a MASTER PASSWORD and DEVICE SECRET to use with AES-256 for encrypting/decrypting.

# Why should you use this?
Have you ever struggled to remember passwords, or have you used the same password for all your logins? This raises a problem for your account security. A password manager works by taking your added entries and stores them into a password vault on a database. Most people will say, "But, won't the database know my passwords and information? Why shouldn't I just keep this on my own device or write them down?". Well when using a password manager, your personal information will be encrypted and hashed over many iterations while it is stored in the database only retrievable when your device secret and master password is given. Even if someone was able to get the passwords while they were in the database they would have to go through (in this case) a million hashes of your original information which could take forever. Now, when utilizing a password manager, you only have to memorize one thing and that is a MASTER PASSWORD only you might know which will let you access all your account logins at your disposale. 

# Known potential problem
If you copy and paste your password when adding an entry, it will return a symbol when attempting to retreive your password. Please try to type your password instead while attempting to add an entry.

# Installation
You need to have python3 to run this on Windows, Linux or MacOS
## Linux
### Install Python Requirements
```
sudo apt install python3-pip
pip install -r requirements.txt
```

### MariaDB
#### Install MariaDB on linux with apt
```
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xcbcb082a1bb943db
sudo add-apt-repository 'deb http://ftp.osuosl.org/pub/mariadb/repo/5.5/ubuntuprecise main'
sudo apt-get update
sudo apt-get install mariadb-server
```
#### Create user 'pm' and grant permissions
**Login to mysql as root**

```
sudo mysql -u root
```
**Create User**
```
CREATE USER 'pm'@localhost IDENTIFIED BY 'password';
```
**Grant privileges**
```
GRANT ALL PRIVILEGES ON *.* TO 'pm'@localhost IDENTIFIED BY 'password';
```

### Pyperclip
[Pyperclip](https://pypi.org/project/pyperclip/) is a python module used to copy data to the clipboard. If you get a "not implemented error", follow this simple fix: https://pyperclip.readthedocs.io/en/latest/index.html#not-implemented-error

## Windows
### Install Python Requirements (type this into command prompt)
```pip install -r requirements.txt```

### MariaDB
#### Install
Follow [these instructions](https://www.mariadbtutorial.com/getting-started/install-mariadb/) to install MariaDB on Windows
#### Create user and grant privileges
- Navigate to MariaDB bin directory
```
C:\Program Files\MariaDB\bin
```
- Login as root with the password you chose while installation
```
mysql.exe -u root -p
```
- Create user
```
CREATE USER 'pm'@localhost IDENTIFIED BY 'password';
```
- Grant privileges
```
GRANT ALL PRIVILEGES ON *.* TO 'pm'@localhost IDENTIFIED BY 'password';
```


## Run
### Configure

You need to first configure the password manager by choosing a MASTER PASSWORD. This config step is only required to be executed once.
```
python config.py make
```
The above command will make a new configuration by asking you to choose a MASTER PASSWORD.
This will generate the DEVICE SECRET, create db and required tables.

```
python config.py delete
```
The above command will delete the existing configuration. Doing this will completely delete your device secret and all your entries and you will loose all your passwords. So be aware!

```
python config.py remake
```
The above command will first delete the existing configuration and create a fresh new configuration by asking you to choose a MASTER PASSWORD, generate the DEVICE SECRET, create the db and required tables.

### Usage
```
python pm.py -h
usage: pm.py [-h] [-s NAME] [-u URL] [-e EMAIL] [-l LOGIN] [--length LENGTH] [-c] option

Description

positional arguments:
  option                (a)dd / (e)xtract / (g)enerate

optional arguments:
  -h, --help            show this help message and exit
  -s NAME, --name NAME  Site name
  -u URL, --url URL     Site URL
  -e EMAIL, --email EMAIL
                        Email
  -l LOGIN, --login LOGIN
                        Username
  --length LENGTH       Length of the password to generate
  -c, --copy            Copy password to clipboard
  -d, --delete          Deletes entry according the 3 user inputs
```


### Add entry
```
python pm.py add -s mysite -u mysite.com -e hello@email.com -l myusername
```
### Delete entry
```
python pm.py delete
```
### Retrieve entry
```
python pm.py extract
```
The above command retrieves all the entries
```
python pm.py e -s mysite
```
The above command retrieves all the entries whose site name is "mysite"
```
python pm.py e -s mysite -l myusername
```
The above command retrieves the entry whose site name is "mysite" and username is "myusername"
```
python pm.py e -s mysite -l myusername --copy
```
The above command copies the password of the site "mysite" and username "myusername" into the clipboard
### Generate Password
```
python pm.py g --length 15
```
The above command generates a password of length 15 and copies to clipboard
