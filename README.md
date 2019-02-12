Ansible

## Ansible installation
```pacman -S ansible```

## Usefull commands
```ansible all -m ping -i hosts -u vagrant``` : Ping all host from the specified inventory **hosts** with the user **vagrant**  

```ansible-playbook -i hosts install.yml``` : Run the playbook to install the server

```ansible-vault encrypt_string -vvv ``` : Encrypt a string (like a password)

```ansible-playbook -i hosts install.yml --flush-cache --ask-vault-pass``` : Run the playbook, and ask for a password to decrypt encrypted passwords.

```ansible-playbook -i hosts install.yml --flush-cache --vault-password-file .vault_pass``` : Run the playbook, with a file who contain a password to decrypt encrypted passwords. You must create this file.
