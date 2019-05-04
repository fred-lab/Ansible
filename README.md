# Ansible

## Pre-requirement
- Debian 9
- Python

## Installation
**Host**  

Clone the repo: ```git clone https://github.com/fred-lab/Ansible.git```  

Install Ansible + Python: ```pacman -S ansible python```

Create **parameters.yml** file : ```cp parameters.yml parameters.yml.dist```  

Create **hosts** file : ```cp hosts.yml hosts.yml.dist```  

Create the .vault-pass file : ```touch .vault_pass```  

Update **hosts** with a server name and an IP adress  

Update **parameters.yml** with adequate information :
- for **superuser**, replace **user** with your username. This user will have access to Root privileges

- for **superuser_ssh_key**, indicate the path to the ssh key for the superuser.

- In the **vault** section, you must replace the values with your passwords. You need a password for the database root access. You can encrypt your password with this command:
```ansible-vault encrypt_string -vvv --vault-password-file .vault_pass ```  

Connect to the Debian 9 serveur with SSH. To run Ansible, the server must have **Python** : ```ssh username@distant_server``` 

**Distant server**

Run the installation : ```ansible-playbook -i hosts install.yml --flush-cache --vault-password-file .vault_pass --force-handler```  

Deploy an app : ```ansible-playbook -i hosts deploy_bokehlicious_prod.yml  --flush-cache --vault-password-file .vault_pass --force-handler```  
 

## User
For security purpose, the logic is to have one superuser with Sudo privilege and create user with no password and no root access to each application. Their names correspond to the name application. 
This users can only access to theirs **home** folder, which contains the application.  
For the database, its the same logic, one user for one database, and the user can only access to his own database.  

## Database  
Connect to the database : ```mysql -uusername -p```  
Check the databases: ```show databases;```  

## Nginx  
Restart Nginx: ```sudo systemctl restart nginx```  
Test the Nginx configuration : ```sudo nginx -t```  
Test the Nginx configuration and display the configuration: ```sudo nginx -T```  

## VIM
Go to insert mode : ```i```  
Echap insert mode : esc button  
Save (not in insert mode ): ```:x```

## Ansible Usefull commands
```ansible all -m ping -i hosts -u vagrant``` : Ping all host from the specified inventory **hosts** with the user **vagrant**  

```ansible-playbook -i hosts install.yml``` : Run the playbook to install the server

```ansible-vault encrypt_string -vvv ``` : Encrypt a string (like a password)

```ansible-vault encrypt_string -vvv --vault-password-file .vault_pass ``` : Encrypt a string (like a password) with a file who contain a password to decrypt encrypted passwords. You must create this file.

```ansible-playbook -i hosts install.yml --flush-cache --ask-vault-pass``` : Run the playbook, and ask for a password to decrypt encrypted passwords.

```ansible-playbook -i hosts install.yml --flush-cache --vault-password-file .vault_pass``` : Run the playbook, with a file who contain a password to decrypt encrypted passwords. You must create this file.
