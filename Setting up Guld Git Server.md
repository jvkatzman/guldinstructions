# Setting up Guld Git Server

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

## Create Digital Ocean server  
* go to https://www.digitalocean.com/
* create ID if necessary
* Click On Droplets and then Create Droplet

cc