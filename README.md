# AlveyChain Validator Setup steps <br><br> jimango's extended version

<i>The core of this tutorial is the very same as the 'Validator node setup script for Alvey Chain' that you can find at https://github.com/AlveyCoin/validator .

Which means this extended guide will still use the codes to collect files and run the validator setup directly from the original AlveyCoin/validator page. My only addition in this fork (copy) is the README file</i> so people with now Ubuntu or linux server experience can easier get their head around how to set up the validator.

<br>

## What I added in this tutorial

I added a few steps at the end to make it easier for others to set up the basic security of the server. And by this make the server a bit more secure.
I do recommend everyone to follow the links just below to learn more about how to improve the security and maybe learn a bit more about how to manouver in an Ubuntu (linux) environment.

<br>

## First

You have already bought a VPS or other similar server environment. If your home computer I suggest you first install Putty to be able to connect and log in to your VPS. You can find Putty at this link: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html .

You should also have been provided your VPS login info, of how to log in with the user root.

<br>
<br>

## Now - Before you Start

You and you alone are responsible for securing the server where your Alvey Validator Node is hosted. 
There are many guides available on the internet and youtube tutorials that can help you through hardening your VPS. Here are just a few examples to get you started but there are many more. 

[Initial Server Setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)
[Securing Your Linux VPS](https://www.digitalocean.com/community/tutorials/an-introduction-to-securing-your-linux-vps)
[Hardening Access to Your Server | Linux Security Tutorial](https://www.youtube.com/watch?v=eeaFoZlSq6I)
[5 Steps to Secure Linux (protect from hackers)](https://www.youtube.com/watch?v=ZhMw53Ud2tY)

<br>

## Private Keys 

When running `./init_validator.sh` at the end of the install process you will see two sets of data that you need to copy. The first set of data will print the Validator address, BLS Public key and the Node ID

`[SECRETS INIT]
Public key (address) = 0xC12bB5d97A35c6919aC77C709d55F6aa60436900
BLS Public key       = 0x9952735ca14734955e114a62e4c26a90bce42b4627a393418372968fa36e73a0ef8db68bba11ea967ff883e429b3bfdf
Node ID              = 16Uiu2HAmVZnsqvTwuzC9Jd4iycpdnHdyVZJZTpVC8QuRSKmZdUrf` 

The Public key (address) and BLS Public key are the ONLY 2 data items you need to give for voting. 

The final step in the install script will print on screen your `Private Key` and prompt saying `please make a copy of your and store this somewhere safe - DO NOT SHARE YOUR PRIVATE KEY WITH ANYONE! Once this is done, press any key to continue.."` 

Once again NEVER SHARE YOUR PRIVATE KEY WITH ANYONE! You can import this private key to metamask to collect earnings from running your validator.

<br>

## Main Validator setup - How to Use

1. SSH into you're newly created VM
2. Run `git clone https://github.com/AlveyCoin/validator && cd validator`
3. Make the setup scripts executable and run them
   1. Run `chmod +x init_validator.sh`
   2. Run `chmod +x start_validator.sh`
   3. Run `./init_validator.sh`
   4. Run `./start_validator.sh`
   
4. Confirm Alvey Node is Running `sudo systemctl status alvey.service`
5. You should see an output like: 

`alvey.service - Alvey Node Service
     Loaded: loaded (/etc/systemd/system/alvey.service; disabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-11-21 19:56:48 UTC; 8s ago` 

<br>

## Minimum system requirements

| Type | Value                                                                                          | Influenced by                                                                                                                |
|------|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| CPU  | 2 cores                                                                                        | <ul><li>Number of JSON-RPC queries</li><li>Size of the blockchain state</li><li>Block gas limit</li><li>Block time</li></ul> |
| RAM  | 2 GB                                                                                           | <ul><li>Number of JSON-RPC queries</li><li>Size of the blockchain state</li><li>Block gas limit</li></ul>                    |
| Disk | <ul><li>10 GB root patition</li><li>30 GB root partition with LVM for disk extension</li></ul> | <ul><li>Size of the blockchain state</li></ul>

<br>
<br>

## Additional steps by Jimango to set up very basic security for your VPS

These following steps are my addition to the origianl tutorial.

<br>

## The steps we will be doing
We want to make some changes to the server to improve the basic security of our server a bit.


1. Setting up a new user & Giving that user admin rights (sudo rights)

2. Remove the remote login access for the root user. Important to have set up the new user first in step 1.


<i>
This is necessary to make the server more secure for getting hacked by malicious login software.
I will provide public links to explain each step.
This way you can also learn how to confirm all steps you do from legitim sources. Always check the source and understand what you do when operating in a server environment.
This will make it safer when taking guidance and advices from others that you can not blindly turst.
</i>

<br>
<br>

## Why
The security of this server is important since it will contain the information required to have control of your validator wallet and setup.

<br>

## First - The new user setup:
In this example we make a user named sammy, you should of course choose some other username that you are comfortable with using.

| Step | Commands                          | Reason                                                                                                                                                  |
|------|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1. | `adduser sammy`                     | Adds new user. At the personal information they ask about you dont add any, just keep pressing Enter until the end.                                     |
| 2. | `usermod -aG sudo sammy`            | This gives your new user admin rights, so that you basically can do similar actions like root but without making your server so volunerable.            |
| 3. | `su sammy`                          | Log into that user. You can see this by how the front of the commandline from now on will show the user name instead of root.                           |
| 4. | `cd`                                | Moves you to the home folder of that new user. When installing applications like the validator setup we want to do that in the new user's home folder.  |
| 5. | `sudo apt update`                   | Check if this new user can check for Ubuntu updates                                                                                                     |
| 6. | `sudo apt upgrade`                  | Install any updates if there is any.                            

If you are able to do all these steps you have set up a new user (which you can log in with using Putty), and this new user have all the rights it needs to install applications like the Alveychain validator setup.

This explaination is similar to that you find in pages like, or similar google searches: 

https://www.digitalocean.com/community/tutorials/how-to-create-a-new-sudo-enabled-user-on-ubuntu-22-04-quickstart

<i>(this is for 22.04 Ubuntu, but its still valid for 20.04 Ubuntu with my explaination above)</i>

Remember to user Ubuntu 20.04 server when setting up the server.

<br>

## Secondly - Remove access for root to log in remotly:
This makes it harder for malicious login software to get access to your server by randomly testing for passwords matching the user root.

Only do this step if you were able to run `sudo apt update` as the new user in earlier steps. If you run the below commands without running the update successful as the new user, you will not be able to log back into your server again after logging out. Which means you would have to reinstall the server and start from scratch. 

| Step | Commands                          | Reason                                                                                                                                                  |
|------|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1. | `sudo pico /etc/ssh/sshd_config`                                                                         | Opens the file sshd_config from its folder. Using the sudo command gives your new user (see above) access to save the file after completion. You can use other editing options like `vi` instead of `pico` but personally I like pico even though I know many prefer vi or similar.       |
| 2a.| Use arrows at the keyboard to search for the `PermitRootLogin yes`and change it to `PermitRootLogin no`  | This removes the posibility for root to log in remotly using putty or similar software.       |
| 2b.| If it says `#PermitRootLogin no` with a # infront you simply remove the #`                                                                         | The sign # in such files deactivate that line, so removing the # will make that command valid. In most cases you dont need to do this step 2b, and you only have to do step 2a. This step is only if that line has a # infront of it. |
| 3. | To close the file after its edited as in step 2, press `Ctrl+X` at the keyboard, follow by pressing `Y` and then `Enter` to save as the same original filename.   | This should bring you out of the file you were editing and back to the command line. Where we can continue our commands. |
| 4. | `sudo systemctl restart sshd` <br> OR <br> `sudo /etc/init.d/sshd restart`                                                   | Each of these commands will restart the loging software so that the changes will be active when login in the next time. If the first does not work, you should try the second. |
| 6. | Next time you log into the server you should no longer be able to login as root, you will need to enter your new username to be able to login to your server for the future.                  | This is a basic setup to improve your servers security        


This explaination is similar to that you find in pages like, or similar google searches: 

https://www.tecmint.com/disable-or-enable-ssh-root-login-and-limit-ssh-access-in-linux/

<br>
<br>

## Switching between new user and root

If you have ran all these previous steps successfully you will no longer be able to log in with root using Putty. You will need to log in using the new user from now on. However once you are logged in you will still be free to switch between the new user and the root user. The following commands explains how to do this.

Switching from new-user to root is done by the command: 

`su` or `su root` 

Switching back to the new user is done by:

`su new-user` 

Each time after switching to the other user (root or new user) you should always do the `cd` command to move into that users home folder.

For example, you log into your server using putty and the new user you set up. Then you want to switch to the root user and it's home folder:

Type `su` and press Enter, type `cd` and press Enter. After this you should be in your root folder where you first started the Validator setup.

The name in the beginning of the line where you write will show the name of the user that you are currently logged in with.

<br>

## Good luck!



