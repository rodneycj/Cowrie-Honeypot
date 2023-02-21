# Cowrie-Honeypot
A school lab on understanding TTP's and to observe the behavior of an actual attacker in a honeypot. 

Abstract: This honeypot demonstration provides an emulation that records the session of an actual attacker. With this session recording, a Hunt Analyst will gain a hands-on understanding of the attackers’ tools, tactics and procedures (TTPs). A term that is increasingly being used in Cyber Defense and Incident Response. 

This lab made use of 2 VM's — an Ubuntu Server 20.04 VM and and a second VM for testing.

Procedure – Detailed Lab Steps 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
CONFIGURING SECURE SHELL (SSH) 

Create a new Ubuntu Server VM with 4GB of ram. Install Secure Shell

     $ yes | sudo apt-get install openssh-server net-tools
     
Modify and save the listening port of SSH from  22 to 22222

     $ sudo vi /etc/ssh/sshd_config
     
Next, run the following commands to restart SSH with the new settings and check its status

     $ sudo systemctl restart sshd
     
     $ sudo systemctl status sshd
     
If the “systemctl status sshd” command comes back saying that sshd is active you have successfully updated your sshd port to 22222. Double-check with the netstat command. 

     $ sudo netstat -plunt | grep 22222
     
This command should show that sshd is listening on port 22222. That is five 2’s. If it does not then something in configuring the ssh port has gone wrong, review the "sshd_config" file and verify that you have saved the correct port for ssh.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------

INSTALLING COWRIE HONEYPOT

First, update the software packages

     $ sudo apt-get update
     
     $ sudo apt-get upgrade
     
Install the packages as dependencies for creating our cowrie honeypot.

     $ yes | sudo apt-get install git python3-virtualenv libssl-dev libffi-dev build-essential libpython3-dev python3-minimal authbind virtualenv vim
     
Add a user named "cowrie", disable the password. When running the, make sure to keep all values their default option and type ‘Y’ when asked to accept the information as being correct.

     $ sudo adduser --disabled-password cowrie
     
Log into the new cowrie account using “su -.”
     
As the cowrie user, use git to download the cowrie honeypot package
     
     $ git clone http://github.com/cowrie/cowrie.git
          
Next, create the virtual sandbox environment for Python and Cowrie to run from
     
     $ cd cowrie; virtualenv cowrie-env
          
Next, activate the Python virtual environment and install the Python packages
     
     $ source cowrie-env/bin/activate
          
     $ python=python3 
          
     (cowrie-env)$ pip install --upgrade pip
          
     (cowrie-env)$ pip install --upgrade -r requirements.txt
          
After the above configuration is complete, the next step is to configure the Cowrie daemon (the process that runs in the background)
     
     (cowrie-env)$ cp etc/cowrie.cfg.dist cowrie.cfg
     
Next, edit the etc/cowrie.cfg configuration file to enable telnet. Change "enabled = false" to "enabled = true". Save and exit the file

     $ nano cowrie.cfg
     
Finally, It’s time to start the cowrie daemon

     (cowrie-env)$ bin/cowrie start
     
Ensure that it is running by using the "netstat" command

     (cowrie-env)$ netstat -plunt | grep 222

----------------------------------------------------------------------------------------------------------------------------------------------------------------------

POST REDIRECTION CONFIGURATION

The last step is to redirect Port 22 and 23 to Ports 2222 and 2223. This may need to be done as superuser, so first exit your session as the cowrie user

     (cowrie-env)$ exit
     
Now, run the "redirect" commands. Each new command starts with the ‘$’ symbol

     $ sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
     
     $ sudo iptables -t nat -A PREROUTING -p tcp --dport 23 -j REDIRECT --to-port 2223
     
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

COWRIE FINAL TESTING

Now, deploy the Cowrie environment for testing. Log back into the cowrie user account to start this process

     $ sudo su – cowrie
     
     $ cd cowrie
     
     $ source cowrie-env/bin/activate
     
Print the cowrie log file, and do NOT close the terminal

     (cowrie-env)$ tail -f var/log/cowrie/cowrie.log
     
On your second VM try connecting to your cowrie VM with ssh or telnet. While doing this make sure you keep the Cowrie VM up so you can see the login attempt coming in 

![090722_HUN200-3](https://user-images.githubusercontent.com/123989567/220241043-8f684182-a7ea-43b6-96b0-e8971f9556ca.jpg)

These logs from the cowrie machine can be opened even if you are not inside the virtual environment. Open the cowrie log file

     $ tail /home/cowrie/cowrie/var/log/cowrie/cowrie.log
