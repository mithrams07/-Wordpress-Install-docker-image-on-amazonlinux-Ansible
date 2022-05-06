# -Wordpress-Installation (docker image)---both-amazonlinux-and-Ubuntu-servers-using-Ansible
Wordpress Installation (using docker image of wordpress) on both Amazon linux and Ubuntu servers using Ansible

## Introduction

## Pre-requests
Installing ansible on manager node.
You can use the below command to install the Ansible in the Manager node.

sudo amazon-linux-extras install ansible2 -y
Once the installation completed. Please check the version throught the below command.

```
[ec2-user@ip-172-31-38-22 ~]$ ansible --version

ansible [core 2.12.5]
  config file = None
  configured module search path = ['/home/ec2-user/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/ec2-user/.local/lib/python3.8/site-packages/ansible
  ansible collection location = /home/ec2-user/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/local/bin/ansible
  python version = 3.8.5 (default, Feb 18 2021, 01:24:20) [GCC 7.3.1 20180712 (Red Hat 7.3.1-12)]
  jinja version = 3.1.2
  libyaml = True
```

## Ansible playbook
vi main.yml
```
---
- name: "Installing Wordpress on amazonlinux"
  hosts: all
  become: true

  vars:
    user_amazon: ec2-user
    wpnet: word-net
    MYSQL_ROOT_PASS: mysqlroot@123
    MYSQL_DB: wordpress
    MYSQL_USR: wpuser
    MYSQL_PASS: wpuser@123

  tasks:
  
    - name: "Install docker on amazon linux"
      yum:
        name:
          - docker
          - python2-pip
        state: present


    - name: "Installing docker client for python on amazon linux"
      pip:
        name: docker-py
        state: present   


    - name: "Restarting and enabling docker"
      service:
        name: docker
        state: restarted
        enabled: yes
        

    - name: "Adding ec2-user to docker group on amazon linux"
      user:
        name: "{{user_amazon}}"
        groups: docker
        append: yes

    - name : "Creating network for wordpress"
      docker_network:
        name : "{{wpnet}}"


    - name: "Create a database container for wordpress"
      docker_container:
        name: database
        image: mysql:5.7
        volumes:
          - mysql-vol:/var/lib/mysql
        env: 
          MYSQL_ROOT_PASSWORD: "{{MYSQL_ROOT_PASS}}"
          MYSQL_DATABASE: "{{MYSQL_DB}}"
          MYSQL_USER: "{{MYSQL_USR}}"
          MYSQL_PASSWORD: "{{MYSQL_PASS}}"
        networks: 
          - name: "{{wpnet}}"


    - name: "Create a wordpress container"
      docker_container:
        name: wordpress
        image: wordpress
        env: 
          WORDPRESS_DB_HOST: database
          WORDPRESS_DB_USER: "{{MYSQL_USR}}"
          WORDPRESS_DB_PASSWORD: "{{MYSQL_PASS}}"
          WORDPRESS_DB_NAME: "{{MYSQL_DB}}"

        volumes:
          - wp-vol:/var/www/html/
        ports:
          - "80:80"
        networks:
          - name: "{{wpnet}}"
```
        
 ![syn](https://user-images.githubusercontent.com/36097660/167087029-9359ba18-ca30-4b67-91a5-cc50dc7d4fb4.png)


        
        
   ## Output will be as below
   ```
   [ec2-user@ip-172-31-38-22 ~]$ ansible-playbook  -i hosts word.yml 

PLAY [Installing Wordpress on amazonlinux] ***************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************
The authenticity of host '172.31.2.143 (172.31.2.143)' can't be established.
ECDSA key fingerprint is SHA256:L9dVplgn5zqEn7ULKoN1UUTNRmdFnB1JX1rdmM+OWME.
ECDSA key fingerprint is MD5:83:aa:fa:8c:db:a7:33:13:5b:9f:2f:2f:4c:01:9a:7e.
Are you sure you want to continue connecting (yes/no)? yes
ok: [172.31.2.143]

TASK [Install docker on amazon linux] ********************************************************************************************************
changed: [172.31.2.143]

TASK [Installing docker client for python on amazon linux] ***********************************************************************************
changed: [172.31.2.143]

TASK [Restarting and enabling docker] ********************************************************************************************************
 changed: [172.31.2.143]

TASK [Adding ec2-user to docker group on amazon linux] ***************************************************************************************
changed: [172.31.2.143]

TASK [Creating network for wordpress] ********************************************************************************************************
changed: [172.31.2.143]

TASK [Create a database container for wordpress] *********************************************************************************************
changed: [172.31.2.143]

TASK [Create a wordpress container] **********************************************************************************************************
changed: [172.31.2.143]

PLAY RECAP ***********************************************************************************************************************************
172.31.2.143               : ok=8    changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
         
![wp](https://user-images.githubusercontent.com/36097660/167084077-f0871e78-b7b6-4f0b-8884-ff28fe70052c.png)

## Conclusion
