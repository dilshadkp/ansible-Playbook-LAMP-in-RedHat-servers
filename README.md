# Ansible Playbook to setup LAMP in RedHat based servers

## Description

This is an Ansible Playbook to install Apache, PHP and MariaDB in RedHat/CentOS based Linux servers.

This script will setup a *phpinfo()* page in /var/www/html/index.php in the server.

## Prerequisites

- Install Ansible in Ansible Master server. [Click here for Ansible Installation steps](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

- Configure Inventory/Hosts file


>Configure inventory file in Ansible Master and add the client server details to inventory as below.

```
[Redhat]
IP-of-client-server	    ansible_user="SSH-user"		ansible_port="SSH-port-of-client"	    ansible_private_key_file="/path/to/ssh-key.pem"
```
>Note that this SSH user should have sudo privileges in the client server without a password.
>You can add the user to the sudoes list without password by executing below command in client server as root:

```shell
echo "<SSH-user> ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
```
- ## Ansible Modules used in this Playbook
  - [yum](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/yum_module.html)
  - [copy](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html)
  - [service](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html)
  
- [with_items](https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html) is used as a loop to restart services

- ## Tasks defined in the Playbook


 - #### Task 1
 >Install Apache, PHP and Mariadb
 
```python    
    - name: "Installing apache,mariadb and PHP"
      yum:
         name:
            - httpd
            - mariadb-server
            - php
            - php-mysql
         state: present
```

 - #### Task 2
>Host a sample page in default document root which displays phpinfo()

```python 
    - name: "Set phpinfo page"
      copy:
        content: "<?php phpinfo(); ?>"
        dest: /var/www/html/index.php
        owner: "{{httpd_owner}}"
        group: "{{httpd_group }}"
```

  - #### Task 3
 >Restart and enable apache and Mariadb services

```python
    - name: "Restarting and enabling httpd and mariadb"
      service: 
        name: "{{item}}"
        state: restarted
        enabled: true
      with_items:
        - httpd
        - mariadb
```
- ## Execution
 - Put both the Playbook and inventory file along with the SSH key to access client server in working directory in the Ansible Master server.
 - Run a syntax check
 ```bash
ansible-playbook -i inventory lamp-in-redhat.yml --syntax-check
```
 - Execute the Playbook
 ```bash
ansible-playbook -i inventory lamp-in-redhat.yml
```

x--------------------x---------------------x---------------------x---------------------x---------------------x---------------------x---------------------x---------------------x
### ⚙️ Connect with Me 

<p align="center">
<a href="mailto:dilshad.lalu@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/dilshadkp/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a> 
<a href="https://www.instagram.com/dilshad_a.k.a_lalu/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://wa.me/%2B919567344212?text=This%20message%20from%20GitHub."><img src="https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white"/></a><br />
