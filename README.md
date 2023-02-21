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
          
Next, activate the Python virtual environment and install the python packages
     
     $ source cowrie-env/bin/activate
          
     $ python=python3 
          
     (cowrie-env)$ pip install --upgrade pip
          
     (cowrie-env)$ pip install --upgrade -r requirements.txt
          
          
     
     
     
