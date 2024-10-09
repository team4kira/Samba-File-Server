# Samba-File-Server Kali Linux

Steps to Create a Samba File Server 

1. Install Samba File Server on Kali Linux: sudo apt-get install samba -y
2. Using an editing tool on /etc/samba/smb.conf: sudo nano /etc/samba/smb.conf
3. Add the following command to the bottom smb.conf:
 
  [Add Below for safe measure make single space]
      
      [Public]
      comment = For Public
      
      browseable = yes
      
      path = /path/of/directory [(the command pwd can be used to establish the folder path you're in)] 
      
      writable = yes
      
      read only = no
      
      force create mode = 0666 [(change to permissions of choosing)]
      
      force directory mode = 0777 [(change to permissions of choosing) for permissions guideline use https://chmod-calculator.com/] 

4. Restart Samba: sudo systemctl restart smbd
5. Chech Status of Samba: sudo systemctl status smbd
6. If status is disabled, enable: sudo systemctl enable smbd
7. Add a username and password for security: sudo smbpasswd -a <username_here> (it will then prompt you to enter a password, enter password)
8. Note if you have UFW installed you must create a rule for Samba access: sudo ufw allow from <IP_Address> to any app Samba
9. To access on Window go to winodws explorer and type in: \\\\<IP_Address>
