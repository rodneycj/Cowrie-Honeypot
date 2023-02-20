# Cowrie-Honeypot
A school lab on understanding TTP's and to observe the behavior of an actual attacker in a honeypot. 

Abstract: This honeypot demonstration provides an emulation that records the session of an actual attacker. With this session recording, a Hunt Analyst will gain a hands-on understanding of the attackers’ tools, tactics and procedures (TTPs). A term that is increasingly being used in Cyber Defense and Incident Response. 

This lab made use of 2 VM's — an Ubuntu Server 20.04 VM and and a second VM for testing.

Procedure – Detailed Lab Steps 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
CONFIGURING SECURE SHELL (SSH) 

Create a new Ubuntu Server VM with 4GB of ram

     $ yes | sudo apt-get install openssh-server net-tools
     
Modify the listening port of SSH from  22 to 22222

     $ sudo vi /etc/ssh/sshd_config
     
Next, run the following commands to restart SSH with the new settings and check its status

     $ sudo systemctl restart sshd
     
     $ sudo systemctl status sshd
