# Wordpress-Installation (docker image)-amazonlinux-using-Ansible
Wordpress Installation (using docker image of wordpress) on Amazon linux using Ansible

## Introduction
Here I am explaining how to automate the process of Installing wordpress on amazon linux server using wordpress and mysql docker image in Linux server using Ansible Playbook.

Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code.

## Pre-requests
Install ansible on manager node.
Use the below command to install the Ansible in the Manager node.

```
$ sudo amazon-linux-extras install ansible2 -y
Installing ansible
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
Cleaning repos: amzn2-core amzn2extra-ansible2 amzn2extra-docker amzn2extra-kernel-5.10
17 metadata files removed
6 sqlite files removed
0 metadata files removed
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
amzn2-core                                                                                                             | 3.7 kB  00:00:00     
amzn2extra-ansible2                                                                                                    | 3.0 kB  00:00:00     
amzn2extra-docker                                                                                                      | 3.0 kB  00:00:00     
amzn2extra-kernel-5.10                                                                                                 | 3.0 kB  00:00:00     
(1/9): amzn2-core/2/x86_64/group_gz                                                                                    | 2.5 kB  00:00:00     
(2/9): amzn2-core/2/x86_64/updateinfo                                                                                  | 472 kB  00:00:00     
(3/9): amzn2extra-ansible2/2/x86_64/updateinfo                                                                         |   76 B  00:00:00     
(4/9): amzn2extra-ansible2/2/x86_64/primary_db                                                                         |  39 kB  00:00:00     
(5/9): amzn2extra-docker/2/x86_64/updateinfo                                                                           | 6.2 kB  00:00:00     
(6/9): amzn2extra-kernel-5.10/2/x86_64/updateinfo                                                                      |  13 kB  00:00:00     
(7/9): amzn2extra-docker/2/x86_64/primary_db                                                                           |  88 kB  00:00:00     
(8/9): amzn2extra-kernel-5.10/2/x86_64/primary_db                                                                      | 8.5 MB  00:00:00     
(9/9): amzn2-core/2/x86_64/primary_db                                                                                  |  62 MB  00:00:01     
Resolving Dependencies
--> Running transaction check
---> Package ansible.noarch 0:2.9.23-1.amzn2 will be installed
--> Processing Dependency: sshpass for package: ansible-2.9.23-1.amzn2.noarch
--> Processing Dependency: python-paramiko for package: ansible-2.9.23-1.amzn2.noarch
--> Processing Dependency: python-keyczar for package: ansible-2.9.23-1.amzn2.noarch
--> Processing Dependency: python-httplib2 for package: ansible-2.9.23-1.amzn2.noarch
--> Processing Dependency: python-crypto for package: ansible-2.9.23-1.amzn2.noarch
--> Running transaction check
---> Package python-keyczar.noarch 0:0.71c-2.amzn2 will be installed
---> Package python2-crypto.x86_64 0:2.6.1-13.amzn2.0.3 will be installed
--> Processing Dependency: libtomcrypt.so.1()(64bit) for package: python2-crypto-2.6.1-13.amzn2.0.3.x86_64
---> Package python2-httplib2.noarch 0:0.18.1-3.amzn2 will be installed
---> Package python2-paramiko.noarch 0:1.16.1-3.amzn2.0.2 will be installed
--> Processing Dependency: python2-ecdsa for package: python2-paramiko-1.16.1-3.amzn2.0.2.noarch
---> Package sshpass.x86_64 0:1.06-1.amzn2.0.1 will be installed
--> Running transaction check
---> Package libtomcrypt.x86_64 0:1.18.2-1.amzn2.0.1 will be installed
--> Processing Dependency: libtommath >= 1.0 for package: libtomcrypt-1.18.2-1.amzn2.0.1.x86_64
--> Processing Dependency: libtommath.so.1()(64bit) for package: libtomcrypt-1.18.2-1.amzn2.0.1.x86_64
---> Package python2-ecdsa.noarch 0:0.13.3-1.amzn2.0.1 will be installed
--> Running transaction check
---> Package libtommath.x86_64 0:1.0.1-4.amzn2.0.1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==============================================================================================================================================
 Package                            Arch                     Version                              Repository                             Size
==============================================================================================================================================
Installing:
 ansible                            noarch                   2.9.23-1.amzn2                       amzn2extra-ansible2                    17 M
Installing for dependencies:
 libtomcrypt                        x86_64                   1.18.2-1.amzn2.0.1                   amzn2extra-ansible2                   409 k
 libtommath                         x86_64                   1.0.1-4.amzn2.0.1                    amzn2extra-ansible2                    36 k
 python-keyczar                     noarch                   0.71c-2.amzn2                        amzn2extra-ansible2                   218 k
 python2-crypto                     x86_64                   2.6.1-13.amzn2.0.3                   amzn2extra-ansible2                   476 k
 python2-ecdsa                      noarch                   0.13.3-1.amzn2.0.1                   amzn2extra-ansible2                    94 k
 python2-httplib2                   noarch                   0.18.1-3.amzn2                       amzn2extra-ansible2                   125 k
 python2-paramiko                   noarch                   1.16.1-3.amzn2.0.2                   amzn2extra-ansible2                   259 k
 sshpass                            x86_64                   1.06-1.amzn2.0.1                     amzn2extra-ansible2                    22 k

Transaction Summary
==============================================================================================================================================
Install  1 Package (+8 Dependent packages)

Total download size: 19 M
Installed size: 110 M
Downloading packages:
(1/9): libtomcrypt-1.18.2-1.amzn2.0.1.x86_64.rpm                                                                       | 409 kB  00:00:00     
(2/9): libtommath-1.0.1-4.amzn2.0.1.x86_64.rpm                                                                         |  36 kB  00:00:00     
(3/9): python-keyczar-0.71c-2.amzn2.noarch.rpm                                                                         | 218 kB  00:00:00     
(4/9): python2-crypto-2.6.1-13.amzn2.0.3.x86_64.rpm                                                                    | 476 kB  00:00:00     
(5/9): python2-ecdsa-0.13.3-1.amzn2.0.1.noarch.rpm                                                                     |  94 kB  00:00:00     
(6/9): python2-httplib2-0.18.1-3.amzn2.noarch.rpm                                                                      | 125 kB  00:00:00     
(7/9): python2-paramiko-1.16.1-3.amzn2.0.2.noarch.rpm                                                                  | 259 kB  00:00:00     
(8/9): sshpass-1.06-1.amzn2.0.1.x86_64.rpm                                                                             |  22 kB  00:00:00     
(9/9): ansible-2.9.23-1.amzn2.noarch.rpm                                                                               |  17 MB  00:00:00     
----------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                          47 MB/s |  19 MB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : sshpass-1.06-1.amzn2.0.1.x86_64                                                                                            1/9 
  Installing : python2-httplib2-0.18.1-3.amzn2.noarch                                                                                     2/9 
  Installing : libtommath-1.0.1-4.amzn2.0.1.x86_64                                                                                        3/9 
  Installing : libtomcrypt-1.18.2-1.amzn2.0.1.x86_64                                                                                      4/9 
  Installing : python2-crypto-2.6.1-13.amzn2.0.3.x86_64                                                                                   5/9 
  Installing : python-keyczar-0.71c-2.amzn2.noarch                                                                                        6/9 
  Installing : python2-ecdsa-0.13.3-1.amzn2.0.1.noarch                                                                                    7/9 
  Installing : python2-paramiko-1.16.1-3.amzn2.0.2.noarch                                                                                 8/9 
  Installing : ansible-2.9.23-1.amzn2.noarch                                                                                              9/9 
  Verifying  : python2-ecdsa-0.13.3-1.amzn2.0.1.noarch                                                                                    1/9 
  Verifying  : libtommath-1.0.1-4.amzn2.0.1.x86_64                                                                                        2/9 
  Verifying  : python2-crypto-2.6.1-13.amzn2.0.3.x86_64                                                                                   3/9 
  Verifying  : ansible-2.9.23-1.amzn2.noarch                                                                                              4/9 
  Verifying  : python-keyczar-0.71c-2.amzn2.noarch                                                                                        5/9 
  Verifying  : libtomcrypt-1.18.2-1.amzn2.0.1.x86_64                                                                                      6/9 
  Verifying  : python2-paramiko-1.16.1-3.amzn2.0.2.noarch                                                                                 7/9 
  Verifying  : python2-httplib2-0.18.1-3.amzn2.noarch                                                                                     8/9 
  Verifying  : sshpass-1.06-1.amzn2.0.1.x86_64                                                                                            9/9 

Installed:
  ansible.noarch 0:2.9.23-1.amzn2                                                                                                             

Dependency Installed:
  libtomcrypt.x86_64 0:1.18.2-1.amzn2.0.1          libtommath.x86_64 0:1.0.1-4.amzn2.0.1         python-keyczar.noarch 0:0.71c-2.amzn2       
  python2-crypto.x86_64 0:2.6.1-13.amzn2.0.3       python2-ecdsa.noarch 0:0.13.3-1.amzn2.0.1     python2-httplib2.noarch 0:0.18.1-3.amzn2    
  python2-paramiko.noarch 0:1.16.1-3.amzn2.0.2     sshpass.x86_64 0:1.06-1.amzn2.0.1            

Complete!
  0  ansible2=latest          enabled      \
        [ =2.4.2  =2.4.6  =2.8  =stable ]
  2  httpd_modules            available    [ =1.0  =stable ]
  3  memcached1.5             available    \
        [ =1.5.1  =1.5.16  =1.5.17 ]
  5  postgresql9.6            available    \
        [ =9.6.6  =9.6.8  =stable ]
  6  postgresql10             available    [ =10  =stable ]
  9  R3.4                     available    [ =3.4.3  =stable ]
 10  rust1                    available    \
        [ =1.22.1  =1.26.0  =1.26.1  =1.27.2  =1.31.0  =1.38.0
          =stable ]
 11  vim                      available    [ =8.0  =stable ]
 18  libreoffice              available    \
        [ =5.0.6.2_15  =5.3.6.1  =stable ]
 19  gimp                     available    [ =2.8.22 ]
 20  docker=latest            enabled      \
        [ =17.12.1  =18.03.1  =18.06.1  =18.09.9  =stable ]
 21  mate-desktop1.x          available    \
        [ =1.19.0  =1.20.0  =stable ]
 22  GraphicsMagick1.3        available    \
        [ =1.3.29  =1.3.32  =1.3.34  =stable ]
 23  tomcat8.5                available    \
        [ =8.5.31  =8.5.32  =8.5.38  =8.5.40  =8.5.42  =8.5.50
          =stable ]
 24  epel                     available    [ =7.11  =stable ]
 25  testing                  available    [ =1.0  =stable ]
 26  ecs                      available    [ =stable ]
 27  corretto8                available    \
        [ =1.8.0_192  =1.8.0_202  =1.8.0_212  =1.8.0_222  =1.8.0_232
          =1.8.0_242  =stable ]
 28  firecracker              available    [ =0.11  =stable ]
 29  golang1.11               available    \
        [ =1.11.3  =1.11.11  =1.11.13  =stable ]
 30  squid4                   available    [ =4  =stable ]
 32  lustre2.10               available    \
        [ =2.10.5  =2.10.8  =stable ]
 33  java-openjdk11           available    [ =11  =stable ]
 34  lynis                    available    [ =stable ]
 35  kernel-ng                available    [ =stable ]
 36  BCC                      available    [ =0.x  =stable ]
 37  mono                     available    [ =5.x  =stable ]
 38  nginx1                   available    [ =stable ]
 39  ruby2.6                  available    [ =2.6  =stable ]
 40  mock                     available    [ =stable ]
 41  postgresql11             available    [ =11  =stable ]
 42  php7.4                   available    [ =stable ]
 43  livepatch                available    [ =stable ]
 44  python3.8                available    [ =stable ]
 45  haproxy2                 available    [ =stable ]
 46  collectd                 available    [ =stable ]
 47  aws-nitro-enclaves-cli   available    [ =stable ]
 48  R4                       available    [ =stable ]
  _  kernel-5.4               available    [ =stable ]
 50  selinux-ng               available    [ =stable ]
 51  php8.0                   available    [ =stable ]
 52  tomcat9                  available    [ =stable ]
 53  unbound1.13              available    [ =stable ]
 54  mariadb10.5              available    [ =stable ]
 55  kernel-5.10=latest       enabled      [ =stable ]
 56  redis6                   available    [ =stable ]
 57  ruby3.0                  available    [ =stable ]
 58  postgresql12             available    [ =stable ]
 59  postgresql13             available    [ =stable ]
 60  mock2                    available    [ =stable ]
 61  dnsmasq2.85              available    [ =stable ]
 ```
Use the below command to check the ansible version.

```
 ansible --version

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
### Adding inventory file and private key fie.

Create a working directory. I have created a directory "wordpress" under /home/ec2-user. 
Under this directory add below files.

#### 1. Inventory file.
   I have created file "hosts" for add the details to connect to the client server as below.
   ```
  [wordpress]
  <IP address of destination server > ansible_user="ec2-user" ansible_port=22 ansible_ssh_private_key_file="ansible.pem"
  ```
 The heading "wordpress" in bracket is group name, which is used in classifying hosts and deciding what hosts you are controlling.
 
#### 2. Private key
 I have created a file "ansible.pem" and pasted the corresponding private key of client server.

Both hosts and private key should be in the working directory where the ansible-playbook file reside.

## Ansible playbook

I have created a YML file inside the working directory named "main.yml".

vi main.yml
```
---
- name: "Installing Wordpress on amazonlinux"
  hosts: wordpress
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
 Use the below command to check whether if there is any syntax mistake in the playbook.
   ```
   ansible-playbook  -i hosts word.yml --syntax-check
   ```
  Output will be look like the screenshot below if there is no syantax mistakes.      
 ![syn](https://user-images.githubusercontent.com/36097660/167087029-9359ba18-ca30-4b67-91a5-cc50dc7d4fb4.png)


        
        
   ## Output will be as below
   
When we run the playbook using below command, each task will be executed in given order.
   ```
   ansible-playbook  -i hosts word.yml 

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

##### Explanation of tasks:
Task 1 : To install docker service in client node.
Task 2 : To installing docker client for python on amazon linux. 
Task 3 : To start and enable docker service.
Task 4 : Adding ec2-user to docker group so that ec-user will be able to execute docker commands.
Task 5 : Creating network. Containers will be attached to this network
Task 6 : Creating database using mysql5.7 mysql docker image for wordpr.ss using the variables mentioned in "vars" section
Task 7 : Create wordpress container using "wordpress" docker image 

After executing playbook successfully, you can access the client server IP address in browser and will load the wordpress installation page like below
         
![wp](https://user-images.githubusercontent.com/36097660/167084077-f0871e78-b7b6-4f0b-8884-ff28fe70052c.png)

## Conclusion
Here, we have discussed about how to automate the wordpress installation with docker image using ansible.This is an basic form of it.
