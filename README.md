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

