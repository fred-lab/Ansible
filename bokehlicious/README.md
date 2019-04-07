# Deploy Bokehlicious

## Pre-requirement
- Git
- SSH Deploy key
- Ansible 2.7.9

## Install Ansistrano  
```ansible-galaxy install ansistrano.deploy ansistrano.rollback```


### Create an SSH key
**On the local host**, create a SSH key : ```ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/deploy_key -C "Deploy key"```  
Start SSH Agent : ```eval "$(ssh-agent -s)"```  
Add this key to the SSH Agent : ```ssh-add ~/.ssh/*```  
Check if the SSH key is added : ```ssh-add -L```  

Copy the public key into the clipbpard : ```xclip -sel clip < deploy_key.pub```  

Add the public key to the git hostname :
- for gitlab, go the the **project** -> **settings** -> **repository** -> **deploy keys** and paste the public key into **key**, add a title and click **add key**  
- for github, go the the **project** -> **settings** ->  **deploy keys** -> **add deploy key** and paste the public key into **key**, add a title and click **add key**

Make sure the path in **deploy_private_key** is correct (into defaults/main.yml)

_Note_ : by default, a deploy key can only read the repository and has access only to this one repository, the task would copy the private key to the server, and allow the script to connect to Git.

## Usage  
The server need to be installed first : ```ansible-playbook -i hosts install.yml --flush-cache --vault-password-file .vault_pass --force-handler ```  

In the **parameters.yml**, you must add your encrypted password to access to the database: ```ansible-vault encrypt_string -vvv --vault-password-file .vault_pass ```  

Deploy with : ```ansible-playbook -i hosts deploy_bokehlicious_prod.yml  --flush-cache --vault-password-file .vault_pass --force-handler```  

Rollback with : ```ansible-playbook -i hosts rollback_bokehlicious_prod.yml  --flush-cache --vault-password-file .vault_pass --force-handler```  

Add ```-vvv``` for more verbosity

## Folders  
Into the task folders :
- **host** contains all the task to update the local server  
- **ansistrano** contains all the task to deploy the app  


The **main.yml** is the main task to handle the sub task of **host** & **ansistrano**.  

Inside **ansistrano**, the folders are organized into sub folders corresponding to the lifecycle of Ansistrano.  
The logic is to separate the different types of backend (symfony, laravel) to have a reusable structure, based on Ansistrano's life cycles.  

## User  
This tasks don't create an user to use the application.
For **Bokehlicous** with **Laravel**, a solution is to use Tinker to create an user.  

## Links  
[Ansistrano](https://github.com/ansistrano/deploy)


