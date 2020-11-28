<div align="center">
    <a href="https://github.com/DevOpsThinkk/Lamp-Ansible-Role-CentOS/blob/master/lamp.png" target="_blank">
        <img alt="LAMP" src="https://github.com/DevOpsThinkk/Lamp-Ansible-Role-CentOS/blob/master/lamp.png">
    </a>
</div>


# CentOS 7 – LAMP Stack
Ansible Playbook for CentOS 8 LAMP server with phpMyAdmin

# Filesystem Layout:

$Home/
$Home/lamp-playbook.yml
$Home/roles/
$Home/roles/apache-server/
$Home/roles/php-stack/
$Home/roles/mariadb-server/
$Home/roles/phpmyadmin/

This playbook consists of several roles. While they could all be combined into a single monolithic role I think it best to have them separated for reuse. 

# Role: apache-server
This role only consists of tasks to install and start apache on the server.

# Role: php-stack
Again, only tasks here with the basics for working php that will support most applications.

# Role: mariadb-server
Since the mariadb-server tasks include variables for the mysql root user, we’ll create an ansible-vault to keep them safe from prying eyes.

```
ansible-vault encrypt mariadb-server/vars/main.yml
New Vault password:
Confirm New Vault password:
```

Now we can edit the file and add the variable for our mysql root user

```
ansible-vault encrypt mariadb-server/vars/main.yml
New Vault password:
Confirm New Vault password:
```
```
---
# vars file for mariadb-server
mysql_root_password: StrongPassword
```
# Role: phpmyadmin

This role consists of two tasks, a handler and a single config file to copy once installed to allow access from outside of the LAMP server.

# lamp-playbook.yml
And now we can put it all together into a playbook to build out the LAMP server.

```
---
- hosts: lamp
  roles:
    - apache-server
    - php-stack
    - mariadb-server
    - phpmyadmin
  become: yes
```

Now, to deploy we issue the following while keeping in mind we have a vault to decrypt at run time:

```
ansible-playbook lamp-playbook --ask-vault-pass
```
