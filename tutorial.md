# Linux Tutorial #
#### By Kian Abdollahi, Set C ####

## This tutorial will cover the following: ##
1. How to create a new regular user that has administrative 
privileges, a password, bash login shell, and SSH access to server

2. How to prevent the root user from connecting to the server via SSH

3. How to install nginx 

4. How to configure nginx to serve a sample website

## Section 1: Creating new regular user ##
Connect to your droplet via root to begin this tutorial

### Create regular user <tutorial-user1> ###

    ```bash
    useradd -ms /bin/bash tutorial-user1
    ```
#### The -m creates a home directory for the new user automatically, the -s takes an argument for the users default shell. In this case we are seting the default login shell to /bin/bash. The users name is tutorial-user1 ####

### Give user <tutorial-user1> a password ###

    ```bash
    passwd tutorial-user1
    ```

![Alt text](image-3.png)

### Add user to sudo group ###

    ```bash
    usermod -aG sudo tutorial-user1
    ```
#### This command will add user <tutorial-user1> to the group sudo, giving them elevated privileges. The -a flag means append, so it will append the user to the group instead of removing them from all other groups. The -G is used to specify the group to add to. ####

### Allow user to access server via ssh ###

#### Copy .ssh directory from the root users home directory to new users home directory ####

    ```bash
    cp -r .ssh/ /home/tutorial-user1/
    ```

#### This copies recursively the .ssh/ folder and its contents from the root home directory to the new users home directory. Replace with the name of your user. ####

![Alt text](image-5.png)
![Alt text](image-6.png)


### Change ownership of .ssh folder to new user ###

    ```bash
    chown -R tutorial-user1:tutorial-user1 .ssh/
    ```

#### This changes the ownership of the .ssh/ directory to the new user. The -R means recursive, and will change ownership of folder and all contents. ####

![Alt text](image-7.png)

### Test you can connect with new user ###

    ```bash
    ssh -i .\.ssh\do-key tutorial-user@147.182.197.18
    ```
    
#### If you can successfully access your server using the new user, you have succeeded. If you get a permission denied, something went wrong. ####

![Alt text](image-8.png)

## Section 2: Edit ssh configuration so can no longer connect via root ###

### Locate the sshd_config file located in /etc/ssh ###
    
![Alt text](image-9.png)

### Edit the sshd_config file so that you can no longer permit root login via SSH (Use sudo if not root) ###

    ```bash
    vim sshd_config
    ```

#### Search the file for the line "PermitRootLogin" ####

![Alt text](image-11.png)

#### Change the "yes", to "no" ####

### Restart the .ssh service to refresh the configuration ###

    ```bash
    systemctl restart ssh.service
    ```   
![Alt text](image-13.png)

### Test that you can no longer connect to the root via ssh. You should see a permissions denied message if you succeeded. ###

![Alt text](image-14.png)

## Section 3: How to install nginx ##

### Update the package manager ###
    ```bash
    sudo apt update
    ```
![Alt text](image-15.png)

### Install nginx ###
    ```bash
    sudo apt install nginx
    ```
### Check the service is running ###
    ```bash
    systemctl status nginx
    ```
#### Make sure the service is active and running ####

![Alt text](image-16.png)