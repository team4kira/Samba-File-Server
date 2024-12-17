# Samba-File-Server Kali Linux

Note: By default, Samba operates within your local network (LAN). This means it relies on IP and subnet configuration to ensure visibility and access are limited to devices on the same network. 

**Steps to Create a Samba File Server**

1. Install Samba File Server on Kali Linux: `sudo apt-get install samba -y`
2. Using an editing tool on /etc/samba/smb.conf: `sudo nano /etc/samba/smb.conf`
3. Add the following command to the bottom of file smb.conf:
 
  [Add Text Below to smb.conf file]
      
      [Public]
      comment = For Public 
      browseable = yes 
      path = /path/of/directory # Go to the directory you want accessed and type in pwd to get the exact path 
      writable = yes 
      read only = no 
      force create mode = 0666 # Change to permissions of choosing 
      force directory mode = 0777 # Change to permissions of choosing for permissions guideline use https://chmod-calculator.com/

4. Restart Samba: `sudo systemctl restart smbd`
5. Chech Status of Samba: `sudo systemctl status smbd`
6. If status is disabled, enable: `sudo systemctl enable smbd`
7. Add a username and password for security: `sudo smbpasswd -a <username_here>` (it will then prompt you to enter a password, enter password)
8. Note if you have Uncomplicated Firewall (UFW) installed you must create a rule for Samba access: `sudo ufw allow from <IP_Address> to any app Samba`
9. To access on Windows go to Windows Explorer and type in: \\\\<IP_Address>
10. Then input you username and password for access to your Samba File Server

* Best practice to make a copy of smb.conf file as a backup, once you have successfully gained access

**How to disable Home Directory Access (Optional)** 

Samba automatically permits Home Directory access. In order to prevent Home Directory Visibility change the following in smb.conf file. `sudo nano /etc/samba/smb.conf`

[Edit the [homes] section of smb.conf to the following]
      
      [homes]
      comment = Home Directories
      available = no
      browseable = yes 
      writable = yes
      valid users = %S # Command Dynamically resolves to the username of the user trying to access the share Still restricts access to authenticated users
      valid users = MYDOMAIN\%S # Acts as a redundancy 

* Anytime changes are made to smb.conf you should restart samba: `sudo systemctl restart smbd`
* To identify what is being shared use the following command: `smbclient -L localhost -U <username_here>`
