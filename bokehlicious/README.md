# Deploy : Bokehlicious

## Pre-requirement
- Git
- SSH Deploy key

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
The server need to be install first : ```ansible-playbook -i hosts install.yml --flush-cache --vault-password-file .vault_pass --force-handler ```  
Deploy with : ```ansible-playbook -i hosts deploy_bokehlicious_prod.yml  --flush-cache --vault-password-file .vault_pass --force-handler```  
