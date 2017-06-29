# Setting up Guld Git Server

**NOTE:** You need to use your terminal command line for some of these steps. 

## The general steps will be:
* Create Digital Ocean server
* Add additional users with sudo rights to server
* Create user with sudo rights so that we don’t use root
* Create home directory for sudo id
* Need to check that git_owner is using bash 
* Create another user that will be used for git o lite and does not have sudo rights
* Now create git user
* Create home directory for sudo id
* Check shell version
* Now set up gitolite
* Test on local machine by cloning gitolite-admin and adding users

-----------------------------------------------

## Create Digital Ocean server  
* go to https://www.digitalocean.com/
* create ID if necessary
* Click On Droplets and then Create Droplet
![create droplet](https://github.com/jvkatzman/guldinstructions/blob/master/images/createdroplet.png?raw=true)

* on the _Distributions_ tab:

* Choose the following minimum criteria for the droplet: Ubunctu/ 16.04.2 x64 , $10/month (1Gb/1 CPU, 30 GB SSD disk, 2 TB transfer)
![create droplet](https://github.com/jvkatzman/guldinstructions/blob/master/images/DropletOptionsP1.png?raw=true)

* Choose  a datacenter regtion
![create droplet](https://github.com/jvkatzman/guldinstructions/blob/master/images/DropletOptionsP2.png?raw=true)

* Add your SSH public key  (we will call it _**gitServer**_ in this example.) If you do not know how to create an SSH see: https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets
![create droplet](https://github.com/jvkatzman/guldinstructions/blob/master/images/DropletOptionsP3.png?raw=true)

* Choose a host name - we used _**gitServer**_
![create droplet](https://github.com/jvkatzman/guldinstructions/blob/master/images/DropletOptionsP4.png?raw=true)

* Press Create.  Note the IP address on the completed droplet
![create droplet](https://github.com/jvkatzman/guldinstructions/blob/master/images/dropletsCompleted.png?raw=true)

* Now test from the terminal to make sure that you can access the server using your ssh key. 

    Open a terminal session and enter (replace _**gitServer**_ and **IP address** with your information):  
__
    > ssh -i ~/.ssh/_**gitServer**_  root@**_104.236.69.202_**

    You will be prompted for a new host and passphrase confirmation. You should see something similar to:

    The authenticity of host '104.236.69.202 (104.236.69.202)' can't be established.
ECDSA key fingerprint is SHA256:fAAANA5Z8TAgL7Ln+WPmvx7YE47AFSSxYURufNoq0M.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '104.236.69.202' (ECDSA) to the list of known hosts.
Enter passphrase for key '/Users/XX/.ssh/gitServer':

-----------------------------------------------

## Create user with sudo rights so that we don’t use root.  We will use **_git_owner_** as an example.
* Now that you are on the server, enter from the terminal:

> useradd **_git_owner_**  
> passwd **_git_owner_**  
> usermod -aG sudo **_git_owner_**


-----------------------------------------------

## Create home directory for sudo id **_git_owner_**. Enter from the terminal:   
> mkdir /home/**_git_owner_**

* change directory owner from root to _**git_owner**_ : 
>chown -R git_owner:**_git_owner_** /home/**_git_owner_**

* change to new user:
> su **_git_owner_**


* confirm user is _**git_owner**_


> whoami

* go to home directory
> cd

    (create an .ssh directory
> mkdir .ssh

* create authorized keys
> nano .ssh/authorized_keys

* Add your ssh public key for super user  - 1 line per key. * * We used the **_git_su_** key)

> Ctrl-X

> Y 

* change rights on file
> chmod 644 .ssh/authorized_keys

-----------------------------------------------

## Need to check that **_git_owner_** user can access the server
    (Open another terminal session and ssh in:)

> ssh -i ~/.ssh/_**git_su**_ _**git_owner**_@104.236.69.202

* Enter password for _**git_owner**_
* If successful, close terminal.
* If not successful, check the .ssh/authorized_keys file

-----------------------------------------------
## Need to check that **_git_owner_** is using bash 
* Back at the  **_git_owner_** terminal, enter:
> echo "$SHELL"

* This should return should return /bin/bash
* if not: go to root user in another terminal 
> chsh -s /bin/bash git_owner
*  now re-login as _**git_owner**_ and test



-----------------------------------------------
## Now create git user
* In the terminal with _**git_owner**_
> sudo useradd git
> sudo passwd git

-----------------------------------------------
## Create home directory for sudo id - we are usint _**git**_
> sudo mkdir /home/**git**
* change owner from root to _**git**_
> sudo chown -R _**git**_:_**git**_ /home/_**git**_
* change to new **git**
> su _**git**_
* Check that home/_**git**_ exists
> cd /home/_**git**_
* create .ssh directory
> sudo mkdir .ssh

-----------------------------------------------
## Create an end user key that will be used for git o lite and does not have sudo rights
* In the terminal with _**root**_
* create a new ssh key - we are using _**gituser**_
* Add end user ssh key to git server in /home/_**git**_/ directory
> sudo nano .ssh/authorized_keys
* Add the new key to the file. Make sure 1 line per key. 
* No extra lines
> Ctrl-X

> Y 
-----------------------------------------------
## Need to check that **_git_** user can access the server
    (Open another terminal session and ssh in:)

> ssh -i ~/.ssh/_**gituser**_ _**git**_@104.236.69.202

* If successful, close terminal.
* If not successful, check the .ssh/authorized_keys file

-----------------------------------------------
## Check shell version for _**git**_
* Back at the  **_git_owner_** terminal, enter:
> su git
* Confirm user
> whoami
* Test for shell
> echo "$SHELL"

* This should return should return /bin/bash
* if not: go to root user in another terminal 
> chsh -s /bin/bash git
*  now re-login as _**git**_ and test


-----------------------------------------------
## Now set up gitolite

* You can also reference: 	
<http://gitolite.com/gitolite/install/index.html#install-and-setup_1>

* Using user **_git_**
* Go to the home/git directory 
> cd /home/git
* create a new ssh key - we are using _**gitadmin**_  on your local machine

* copy over gitadmin.pub
> nano gitadmin.pub

* copy key from local machine and save

* clone the gitolite code:
> git clone git://github.com/sitaramc/gitolite

* results will look similar to:
Cloning into 'gitolite'...
remote: Counting objects: 9381, done.
remote: Total 9381 (delta 0), reused 0 (delta 0), pack-reused 9381
Receiving objects: 100% (9381/9381), 2.96 MiB | 0 bytes/s, done.
Resolving deltas: 100% (5803/5803), done.
Checking connectivity... done.

* create a bin directory
> mkdir bin

* Install the software

> gitolite/install -ln 
* Check that install worked
> ls /bin -la
* set up key
> /home/git/bin/gitolite setup -pk gitadmin.pub

-----------------------------------------------
## Test on local machine by cloning gitolite-admin and adding users
-----------------------------------------------


cc