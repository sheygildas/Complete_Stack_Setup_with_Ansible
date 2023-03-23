
# Project-11
## :ledger: Index

- [About The Project](#beginner-about-the-project)
- [Problem that this project solves ](#question-problem-that-this-project-solves)
- [Solution to the problem.](#key-solution-to-the-problem)
- [Tools and Services](#hammer_and_wrench-tools-and-services)
- [Architecture of this project](#house-architecture-of-this-project)
- [Steps to execute the project](#zap-steps-to-execute-the-project)
  - [Login to AWS Account ](#key-login-to-aws-account)
  - [Create ec2 instance to run ansible playbook](#package-create-ec2-instance-to-run-ansible-playbook)
  - [Install Ansible](#package-install-ansible)
  - [Install boto](#package-install-boto)
  - [Setup ec2 Role for ansible](#package-setup-ec2-role-for-ansible)
  - [Create a project directory](#package-create-a-project-directory)
  - [Execute a sample cloud task](#package-execute-a-sample-cloud-task)
  - [Create Variables File for VPC & Bastion host](#package-create-variables-file-for-vpc-and-bastion-host)
  - [Create VPC Setup Playbook](#package-create-vpc-setup-playbook)
  - [Create Bastion setup playbook](#package-create-bastion-setup-playbook)
  - [Site.yml playbook to call both playbook at once](#package-siteyml-playbook-to-call-both-playbook-at-once)
  - [Playbooks for vprofile stack setup](#package-playbooks-for-vprofile-stack-setup)	
    - [Create Security Group Playbook](#package-create-group-playbook)
    - [Create Database Playbook](#package-create-database-playbook)
    - [Create Memcached Playbook](#package-create-memcached-playbook)
    - [Create RabbitMQ Playbook](#package-create-rabbitmq-playbook)
    - [Create Tomcat Playbook](#package-create-tomcat-playbook)
    - [Create Nginx Playbook](#package-create-nginx-playbook)
    - [Create Load Balancer Playbook](#package-create-load-balancer-playbook)
    - [Dynamic Inventory and Dynamic variables Playbook](#package-dynamic-inventory-and-dynamic-variables-playbook)
- [Resources](#page_facing_up-resources)
- [Credit/Acknowledgment](#star2-creditacknowledgment)



## :beginner:About The Project

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

## :question: Problem that this project solves 

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

## :key: Solution to the problem.

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

## :hammer_and_wrench: Tools and Services
- visual studio code or an IDE
- AWS Account
- GitHub account
- Git

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>


## :beginner: Architecture of this project.

![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

## :zap: Steps to Execute the project. 

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :key: Login to AWS Account

- Login to your AWS management console and choose ohio region for this project. Create an AWS account if you don't have one. 

 ```sh
Account ID (12 digits) or account alias: <Enter your credentials>
IAM user name: <Your IAM user name>
Password: <Your password>
   ```
   
<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>



### :package: Create ec2 instance to run ansible playbook

- On your console under `EC2`->`Instances` click `launch instances`.
- We will create ec2 instance to run ansible playbook with below details.

 ```sh
Name: Control-Machine
AMI: Ubuntu 20 and above
Instance Type: t2.micro
Security Group Nmae: Ansible-sg
Security group rules: allow SSH on port 22 from MYIP
 key pair: Ansible-ohio-key
   ```
- Let;s launch our Ansible machine with some userdata as given below.

```sh
#!/bin/bash
apt update
apt install ansible -y
   ```
   
- SSH into your Ansible machine check your version of ansible .

 ```sh
ssh -i <directory of your key pair/key pair> ubuntu
ansible --version
   ```
   

     
<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Install Ansible Manually 

- If you are using ubuntu 18 you have to install ansible manually from the ansible repository.
- Checkout [ansible AWS documentation here](https://ansible-docs.readthedocs.io/zh/stable-2.0/rst/guide_aws.html)

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>


### :package: Setup ec2 Role for ansible
   
- On the console, search for  EC2 instance service. Goto `Actions` --> `Security` --> `Modify IAM Role`
- Create an IAM role for our Ansible machine to use with the following details.

```sh
Service: EC2
Policy: AdministratorAccess
Role name: ansible-admin
   ```
- Install AWS CLI on your ansible machine and test your authentication using the command below 


```sh
sudo apt install awscli -y
aws sts get-caller-identity
   ```

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Create a project directory
- On your Ansible machine, create a new directory with the following details.

```sh
mkdir vpc-stack-vprofile
cd vpc-stack-vprofile
vim test-aws.yml
   ```
   
<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Execute a sample cloud task 
- Checkout [ansible AWS documentation here](https://docs.ansible.com/ansible/2.9/modules/list_of_cloud_modules.html) we coppied our sample cloud task below from there.
- Open the file `test-aws.yml` with this command `vim test-aws.yml` paste the content below.Make sure you take care of the indentation

```sh
- hosts: localhost
  connection: local
  gather_facts: False
  tasks:
    - name: sample ec2 key
      ec2_key:
        name: my_keypair
        region: us-east-2
   ```
- Try runing this playbook with the command `ansible-playbook test-aws.yml`, you will notice that it faills because python3 is not installed on your Ansible machine.

![Project Image](project-image-url) 

### :package: Install boto

- On AWS bot is the SDK for python.
- Lt's install bot with the following command 
- 
```sh
sudo apt search boto
sudo apt install python3-boto3 -y
   ```
- Run the playbook again,  you will notice that it is successfull because boto3 is installed.

- Add more tasks to our `test-aws.yml` playbook as given below.

```sh
- hosts: localhost
  connection: local
  gather_facts: False
  tasks:
    - name: sample ec2 key
      ec2_key:
        name: my_keypair
        region: us-east-2
      register: keyout

    - debug:
        var: keyout

    - name: store login key
      copy:
        content: "{{keyout.key.private_key}}"
        dest: ./sample-key.pem
      when: keyout.changed
   ```
   
- Our key pair is now created and store stored. In our first task, the key pair was created but not store by adding the above lines to our code, we can now store our key pair. 


<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Create Variables File for VPC and Bastion host

- Login into your github account.
- Let's create a github repository were our ansible-playbooks will be store. Called the repo `ansible-aws-vpc`

- On your IDE, create 2 two variables file `vpc_setup` and s `bastion_setup`.



Inside the `vpc_setup file`, copy and paste the code below.


```sh
vpc_name: "Vprofile-vpc"

#VPC Range
vpcCidr: '172.20.0.0./16'

#Subnets Range
PubSub1Cidr: 172.20.1.0/24
PubSub2Cidr: 172.20.2.0/24
PubSub3Cidr: 172.20.3.0/24
PrivSub1Cidr: 172.20.4.0/24
PrivSub2Cidr: 172.20.5.0/24
PrivSub3Cidr: 172.20.6.0/24

#Region Name
region: "us-east-2"

#Zone Names
zone1: us-east-2a
zone2: us-east-2b
zone3: us-east-2c

state: present
   ```
   
- Inside the `bastion_setupe`, copy and paste the code below.

```sh
bastion_ami: ami-0beaa649c482330f7 # Amazon Linux-2 AMI-ID from us-east-2 region
region: us-east-2
MYIP: IP_address_of_your_laptop/32
keyName: vprofile-key
instanceType: t2.micro
   ```
   
- Commit and push these two files to GitHub, also clone this repository in our Ansible machine on AWS
 
<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Create VPC Setup Playbook

- Create a `vpc_setup.yml` playbook with the details below

```sh
- hosts: localhost
  connection: local
  gather_facts: False
  tasks:
    - name: Import VPC Variables
      include_vars: vars/vpc_setup

    - name: Create Vprofile VPC
      ec2_vpc_net:
        name: "{{vpc_name}}"
        cidr_block: "{{vpcCidr}}"
        region: "{{region}}"
        dns_support: yes
        dns_hostnames: yes
        tenancy: default
        state: "{{state}}"
      register: vpcout
   ```
   
- Commit and push this file to GitHub, also pull this file in our Ansible machine on AWS

- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpc_setup.yml

   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for VPC service to view changes


<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

#### :package: Create a Subnet Playbook

- On your IDE add the following subnet code to your `vpc_setup.yml` playbook


```sh
#    - debug:
#        var: vpcout

    - name: create Public Subnet 1 in Zone1
      ec2_vpc_subnet:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        az: "{{zone1}}"
        state: "{{state}}"
        cidr: "{{PubSub1Cidr}}"
        map_public: yes
        tags:
            Name: vprofile-pubsub1
      register: pubsub1_out

    - name: create Public Subnet 2 in Zone2
      ec2_vpc_subnet:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        az: "{{zone2}}"
        state: "{{state}}"
        cidr: "{{PubSub2Cidr}}"
        map_public: yes
        tags:
          Name: vprofile-pubsub2
      register: pubsub2_out

    - name: create Public Subnet 3 in Zone3
      ec2_vpc_subnet:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        az: "{{zone3}}"
        state: "{{state}}"
        cidr: "{{PubSub3Cidr}}"
        map_public: yes
        tags:
          Name: vprofile-pubsub3
      register: pubsub3_out

    - name: create Private Subnet 1 in Zone1
      ec2_vpc_subnet:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        az: "{{zone1}}"
        state: "{{state}}"
        cidr: "{{PrivSub1Cidr}}"
        map_public: yes
        tags:
            Name: vprofile-privsub1
      register: privsub1_out

    - name: create Private Subnet 2 in Zone2
      ec2_vpc_subnet:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        az: "{{zone2}}"
        state: "{{state}}"
        cidr: "{{PrivSub2Cidr}}"
        map_public: yes
        tags:
          Name: vprofile-privsub2
      register: privsub2_out

    - name: create Private Subnet 3 in Zone3
      ec2_vpc_subnet:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        az: "{{zone3}}"
        state: "{{state}}"
        cidr: "{{PrivSub3Cidr}}"
        map_public: yes
        tags:
          Name: vprofile-privsub3
      register: privsub3_out
   ```
   
- Commit and push this file to GitHub, also pull this file in our Ansible machine on AWS

- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpc_setup.yml
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for VPC service to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

#### :package: Create Internet Gateway and  Public Route Table Playbook


- On your IDE add the following internet gateway and  Public route table code to your `vpc_setup.yml` playbook

```sh
 - name: Internet Gateway Setup
      ec2_vpc_igw:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        state:  "{{state}}"
        tags:
          Name: vprofile-igw
      register: igw_out

    - name: Setup Public Subnet Route Table
      ec2_vpc_route_table:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        tags:
          Name: Vprofile-PubRT
        subnets:
            - "{{ pubsub1_out.subnet.id }}"
            - "{{ pubsub2_out.subnet.id }}"
            - "{{ pubsub3_out.subnet.id }}"
        routes:
          - dest: 0.0.0.0/0
            gateway_id: "{{ igw_out.gateway_id }}"
      register: pubRT_out
   ```
   
   
   
  


- Commit and push this file to GitHub, also pull this file in our Ansible machine on AWS

- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpc_setup.yml
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for VPC service to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

#### :package: Create a NAT Gateway and Private Route Table Playbook


- On your IDE add the following NAT gateway and private route table code to your `vpc_setup.yml` playbook


```sh
 - name: NAT Gateway1 Setup and allocate new EIP if NATGW does not exist yet in the subnet
      ec2_vpc_nat_gateway:
        subnet_id: "{{ pubsub1_out.subnet.id }}"
        region: "{{region}}"
        state:  "{{state}}"
        wait: yes
        if_exist_do_not_create: yes
      register: natgw1_out

    - name: Setup Private Subnet Route Table
      ec2_vpc_route_table:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        tags:
          Name: Vprofile-PrivRT1
        subnets:
            - "{{ privsub1_out.subnet.id }}"
            - "{{ privsub2_out.subnet.id }}"
        routes:
          - dest: 0.0.0.0/0
            gateway_id: "{{ natgw1_out.nat_gateway_id }}"
      register: privRT1_out

    - name: NAT Gateway3 Setup and allocate new EIP if NATGW does not exist yet in the subnet
      ec2_vpc_nat_gateway:
        subnet_id: "{{ pubsub3_out.subnet.id }}"
        region: "{{region}}"
        state:  "{{state}}"
        wait: yes
        if_exist_do_not_create: yes
      register: natgw3_out

    - name: Setup Private Subnet Route Table2
      ec2_vpc_route_table:
        vpc_id: "{{vpcout.vpc.id}}"
        region: "{{region}}"
        tags:
          Name: Vprofile-PrivRT3
        subnets:
            - "{{ privsub3_out.subnet.id }}"
        routes:
          - dest: 0.0.0.0/0
            gateway_id: "{{ natgw3_out.nat_gateway_id }}"
      register: privRT3_out

   ```




- Commit and push this file to GitHub, also pull this file in our Ansible machine on AWS

- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpc_setup.yml
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for VPC service to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Store ID's to Variables file

- On your IDE add the following code to Store ID's to Variables to your `vpc_setup.yml` playbook


```sh

    - debug:
        var: "{{ item }}"
      loop:
        - vpcout.vpc.id
        - pubsub1_out.subnet.id
        - pubsub2_out.subnet.id
        - pubsub3_out.subnet.id
        - privsub1_out.subnet.id
        - privsub2_out.subnet.id
        - privsub3_out.subnet.id
        - igw_out.gateway_id
        - pubRT_out.route_table.id
        - natgw1_out.nat_gateway_id
        - privRT1_out.route_table.id
        - natgw3_out.nat_gateway_id
        - privRT3_out.route_table.id

    - set_fact:
        vpcid: "{{ vpcout.vpc.id }}"
        pubsub1id: "{{ pubsub1_out.subnet.id }}"
        pubsub2id: "{{ pubsub2_out.subnet.id }}"
        pubsub3id: "{{ pubsub3_out.subnet.id }}"
        privsub1id: "{{ privsub1_out.subnet.id }}"
        privsub2id: "{{ privsub2_out.subnet.id }}"
        privsub3id: "{{ privsub3_out.subnet.id }}"
        igwid: "{{ igw_out.gateway_id }}"
        pubRTid: "{{ pubRT_out.route_table.id }}"
        natgw1id: "{{ natgw1_out.nat_gateway_id }}"
        privRT1id: "{{ privRT1_out.route_table.id }}"
        natgw3id: "{{ natgw3_out.nat_gateway_id }}"
        privRT3id: "{{ privRT3_out.route_table.id }}"
        cacheable: yes

    - name: Create variables file for vpc Output
      copy:
        content: "vpcid: {{ vpcout.vpc.id }}\npubsub1id: {{ pubsub1_out.subnet.id }}\npubsub2id: {{ pubsub2_out.subnet.id }}\npubsub3id: {{ pubsub3_out.subnet.id }}\nprivsub1id: {{ privsub1_out.subnet.id }}\nprivsub2id: {{ privsub2_out.subnet.id }}\nprivsub3id: {{ privsub3_out.subnet.id }}\nigwid: {{ igw_out.gateway_id }}\npubRTid: {{ pubRT_out.route_table.id }}\nnatgw1id: {{ natgw1_out.nat_gateway_id }}\nprivRT1id: {{ privRT1_out.route_table.id }}\nnatgw3id: {{ natgw3_out.nat_gateway_id }}\nprivRT3id: {{ privRT3_out.route_table.id }}"

        dest: vars/output_vars
   ```
   
   
- - RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpc_setup.yml
   ```
   
  ![Project Image](project-image-url)
  
- Run the following command on your AWS Ansible machine to view that out put file.

```sh
cat vars/output_vars
   ```
   
  ![Project Image](project-image-url)
  
<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>


### :package: Create Bastion setup playbook

- Create a `bastion-host.yml` playbook with the details below

```sh
---
- name: Setup Vprofile Bastion Host
  hosts: localhost
  connection: local
  gather_facts: no
  tasks:
    - name: Import VPC setup variable
      include_vars: vars/bastion_setup

    - name: Import VPC setup Variable
      include_vars: vars/output_vars

    - name: Create vprofile ec2 key
      ec2_key:
        name: "{{ keyName }}"
        region: "{{ region }}"
      register: key_out

    - name: Save private key into file bastion-key.pem
      copy:
        content: "{{ key_out.key.private_key }}"
        dest: "./bastion-key.pem"
        mode: 0600
      when: key_out.changed

    - name: Create Sec Grp for Bastion Host
      ec2_group:
        name: Bastion-host-sg
        description: Allow port 22 from everywhere and all port within sg
        vpc_id: "{{ vpcid }}"
        region: "{{ region }}"
        rules:
          - proto: tcp
            from_port: 22
            to_port: 22
            cidr_ip: "{{ MYIP }}"
      register: BastionSG_out

    - name: Creating Bastion Host
      ec2:
        key_name: "{{ keyName }}"
        region: "{{ region }}"
        instance_type: "{{ instanceType }}"
        image: "{{ bastion_ami }}"
        wait: yes
        wait_timeout: 300
        instance_tags:
          Name: "Bastion_host"
          Project: Vprofile
          Owner: DevOps Team
        exact_count: 1
        count_tag:
          Name: "Bastion_host"
          Project: Vprofile
          Owner: DevOps Team
        group_id: "{{ BastionSG_out.group_id }}"
        vpc_subnet_id: "{{ pubsub3id }}"
        assign_public_ip: yes
      register: bastionHost_out

    - name: Creating EC2 in privsub1
      ec2:
        key_name: "{{ keyName }}"
        region: "{{ region }}"
        instance_type: "{{ instanceType }}"
        image: "{{ bastion_ami }}"
        wait: yes
        wait_timeout: 300
        instance_tags:
          Name: "privsub1Instance"
          Project: Vprofile
          Owner: DevOps Team
        exact_count: 1
        count_tag:
          Name: "privsub1Instance"
          Project: Vprofile
          Owner: DevOps Team
        vpc_subnet_id: "{{ privsub1id }}"
        assign_public_ip: yes
      register: privsub1Instance_out

    - name: Creating EC2 in privsub3
      ec2:
         key_name: "{{ keyName }}"
         region: "{{ region }}"
         instance_type: "{{ instanceType }}"
         image: "{{ bastion_ami }}"
         wait: yes
         wait_timeout: 300
         instance_tags:
           Name: "privsub3Instance"
           Project: Vprofile
           Owner: DevOps Team
         exact_count: 1
         count_tag:
           Name: "privsub3Instance"
           Project: Vprofile
           Owner: DevOps Team
         vpc_subnet_id: "{{ privsub3id }}"
         assign_public_ip: yes
         group_id: "{{ BastionSG_out.group_id }}"
      register: privsub3Instance_outGo to Ansible server. First Git pull, then run the playbook.Check BastionHost from AWS console.

   ```

- Commit and push this file to GitHub, also pull this file in our Ansible machine on AWS

- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook bastion-host.yml
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for EC2 service to view changes

-![Project Image](project-image-url)


<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Site.yml playbook to call both playbook at once

- Create a `site.yml` playbook to join both vpc-setup.yml and bastion-host.yml with the details below

```sh
---
- import_playbook: vpc-setup.yml
- import_playbook: bastion-host.yml
   ```
   
 - Commit and push this file to GitHub, also pull this file in our Ansible machine on AWS

- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook site.yml
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for VPC service to view changes

-![Project Image](project-image-url)
<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>

#### :package: Create Variables File for vprofile stack setup

- On your IDE, create a variables file name it `vprostacksetup`. This variable fill will contain all the AMI ID that we will use for our project  



Inside the `vprostacksetup`, search for current AMI ID and put in the file. Mine is as below.


```sh
nginx_ami: ami-07efac79022b86107
tomcat_ami: ami-07efac79022b86107
memcache_ami: ami-07efac79022b86107
rmq_ami: ami-07efac79022b86107
mysql_ami: ami-07efac79022b86107

   ```
   
Let's also join the two variab;e file `vpc_setup` and `bastion_setup` we created earlier in to one variable file and name the file `vpc_setup` 
   
```sh
vpc_name: "Vprofile-vpc"

# VPC Range
vpcCidr: '172.20.0.0/16'

# Subnets Range
PubSub1Cidr: 172.20.1.0/24
PubSub2Cidr: 172.20.2.0/24
PubSub3Cidr: 172.20.3.0/24
PrivSub1Cidr: 172.20.4.0/24
PrivSub2Cidr: 172.20.5.0/24
PrivSub3Cidr: 172.20.6.0/24

# Region Name
region: "us-east-2"

# Zone Names
zone1: us-east-2a
zone2: us-east-2b
zone3: us-east-2c

state: present

# Bastion Vars
bastion_ami: ami-03657b56516ab7912
MYIP: 183.83.39.203/32

   ```
   
<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>

### :package: Playbooks for vprofile stack setup

- Will create playbooks to set up our entire vprofile stack.

- Let's call this playbook `vpro_ec2_stack`. Create the file with the following content. 


```sh
---
- name: Setup Vprofile Stack
  hosts: localhost
  connection: local
  gather_facts: no
  tasks:
    - name: Import VPC setup Variable
      include_vars: vars/vpc-output_vars

    - name: Import vprofile setup Variable
      include_vars: vars/vprostacksetup

    - name: Create vprofile ec2 key
      ec2_key:
        name: vprokey
        region: "{{region}}"
      register: vprokey_out

    - name: Save private key into file loginkey_vpro.pem
      copy:
        content: "{{vprokey_out.key.private_key}}"
        dest: "./loginkey_vpro.pem"
        mode: 0600
      when: vprokey_out.changed

   ```

- Commit and push this file to GitHub, also pull this file in our Ansible machine on AWS. If you have any confidential file that you don't want to push to github, remember to create a git ingnore file and add the extention of the file to keep the file private 

- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpro_ec2_stack
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for EC2 service then key pair to view changes

-![Project Image](project-image-url)



<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>


#### :package: Create Security Group Playbook


- Let's add the following code to our  `vpro_ec2_stack` playbook. This will create the security group for our instance. 


```sh
- name: Create Securiry Group for Load Balancer
      ec2_group:
        name: vproELB-sg
        description: Allow port 80 from everywhere and all port within sg
        region: "{{region}}"
        vpc_id: "{{vpcid}}"
        rules:
          - proto: tcp
            from_port: 80
            to_port: 80
            cidr_ip: 0.0.0.0/0
      register: vproELBSG_out

    - name: Create Securiry Group for Vprofile Stack
      ec2_group:
        name: vproStack-sg
        description: Allow port 22 from everywhere and all port within sg
        region: "{{region}}"
        vpc_id: "{{vpcid}}"
        purge_rules: no
        rules:
          - proto: tcp
            from_port: 80
            to_port: 80
            group_id: "{{vproELBSG_out.group_id}}"

          - proto: tcp
            from_port: 22
            to_port: 22
            group_id: "{{BastionSGid}}"
      register: vproStackSG_out

    - name: Update Securiry Group with its own sg id
      ec2_group:
        name: vproStack-sg
        description: Allow port 22 from everywhere and all port within sg
        region: "{{region}}"
        vpc_id: "{{vpcid}}"
        purge_rules: no
        rules:
          - proto: all
            group_id: "{{vproStackSG_out.group_id}}"

   ```

- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpro_ec2_stack
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for EC2 service an security group to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>

#### :package: Create Database Playbook


- Let's add the following code tou our  `vpro_ec2_stack` playbook. This will create the instances for our project. 


```sh

    - name: Creating Mysql db01
      ec2:
        key_name: vprokey
        region: "{{region}}"
        instance_type: t2.micro
        image: "{{mysql_ami}}"
        wait: yes
        wait_timeout: 300
        instance_tags:
          Name: "db01"
          Project: Vprofile
          Owner: DevOps Team
        exact_count: 1
        count_tag:
          Name: "db01"
          Project: Vprofile
          Owner: DevOps Team
        group_id: "{{vproStackSG_out.group_id}}"
        vpc_subnet_id: "{{privsub1id}}"
      register: db01_out

    - debug:
        var: db01_out.tagged_instances[0].id

   ```


- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpro_ec2_stack
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for EC2 service to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>


#### :package: Create Memcached Playbook


- Let's add the following code tou our  `vpro_ec2_stack` playbook. This will create the instances for our project. 


```sh

    - name: Creating memcache mc01
      ec2:
        key_name: vprokey
        region: "{{region}}"
        instance_type: t2.micro
        image: "{{memcache_ami}}"
        wait: yes
        wait_timeout: 300
        instance_tags:
          Name: "mc01"
          Project: Vprofile
          Owner: DevOps Team
        exact_count: 1
        count_tag:
          Name: "mc01"
          Project: Vprofile
          Owner: DevOps Team
        group_id: "{{vproStackSG_out.group_id}}"
        vpc_subnet_id: "{{privsub1id}}"
      register: mc01_out

   ```


- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpro_ec2_stack
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for EC2 service to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>


#### :package: Create RabbitMQ Playbook



- Let's add the following code tou our  `vpro_ec2_stack` playbook. This will create the instances for our project. 


```sh

    - name: Creating RabbitMQ rmq01
      ec2:
        key_name: vprokey
        region: "{{region}}"
        instance_type: t2.micro
        image: "{{rmq_ami}}"
        wait: yes
        wait_timeout: 300
        instance_tags:
          Name: "rmq01"
          Project: Vprofile
          Owner: DevOps Team
        exact_count: 1
        count_tag:
          Name: "rmq01"
          Project: Vprofile
          Owner: DevOps Team
        group_id: "{{vproStackSG_out.group_id}}"
        vpc_subnet_id: "{{privsub1id}}"
      register: rmq01_out

   ```


- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpro_ec2_stack
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for EC2 service to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>


#### :package: Create Tomcat Playbook


- Let's add the following code tou our  `vpro_ec2_stack` playbook. This will create the instances for our project. 


```sh

    - name: Creating tomcat app01
      ec2:
        key_name: vprokey
        region: "{{region}}"
        instance_type: t2.micro
        image: "{{tomcat_ami}}"
        wait: yes
        wait_timeout: 300
        instance_tags:
          Name: "app01"
          Project: Vprofile
          Owner: DevOps Team
        exact_count: 1
        count_tag:
          Name: "app01"
          Project: Vprofile
          Owner: DevOps Team
        group_id: "{{vproStackSG_out.group_id}}"
        vpc_subnet_id: "{{privsub1id}}"
      register: app01_out

   ```


- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpro_ec2_stack
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for EC2 service to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>


#### :package: Create Nginx Playbook


- Let's add the following code tou our  `vpro_ec2_stack` playbook. This will create the instances for our project. 


```sh
- name: Creating Nginx web01
      ec2:
        key_name: vprokey
        region: "{{region}}"
        instance_type: t2.micro
        image: "{{nginx_ami}}"
        wait: yes
        wait_timeout: 300
        instance_tags:
          Name: "web01"
          Project: Vprofile
          Owner: DevOps Team
        exact_count: 1
        count_tag:
          Name: "web01"
          Project: Vprofile
          Owner: DevOps Team
        group_id: "{{vproStackSG_out.group_id}}"
        vpc_subnet_id: "{{privsub1id}}"
      register: web01_out

   ```


- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpro_ec2_stack
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for EC2 service to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>



#### :package: Create Load Balancer Playbook


- Let's add the following code tou our  `vpro_ec2_stack` playbook. This will create the load balancer for our vfrofile stack. 


```sh
- local_action:
        module: ec2_elb_lb
        name: "vprofile-elb"
        region: "{{region}}"
        state: present
        instance_ids:
          - "{{ web01_out.tagged_instances[0].id }}"
        purge_instance_ids: true
        security_group_ids: "{{ vproELBSG_out.group_id }}"
        subnets:
          - "{{ pubsub1id }}"
          - "{{ pubsub2id }}"
          - "{{ pubsub3id }}"
        listeners:
          - protocol: http # options are http, https, ssl, tcp
            load_balancer_port: 80
            instance_port: 80
            
   ```


- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpro_ec2_stack
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for load balancer service to view changes

-![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>


#### :package:  Dynamic Inventory and Dynamic variables

- Before storing all the public and private IP addresses of our instances, let's create two directories where these variable will be stored.
- In your IDE, create a directory and name it `provision-stack`create another one called `group_vars. This second directory should be created inside the `provision-stack` directory.

- Let's add the following code tou our  `vpro_ec2_stack` playbook. This will create dynamic inventory and dynamic variables for our vfrofile stack. 


```sh
- name: Insert/Update Hosts IP & Name in file provision-stack/group_vars/hostsip
      blockinfile:
        path: provision-stack/group_vars/hostsip
        block: |
          web01_ip: {{ web01_out.tagged_instances[0].private_ip }}
          app01_ip: {{ app01_out.tagged_instances[0].private_ip }}
          rmq01_ip: {{ rmq01_out.tagged_instances[0].private_ip }}
          mc01_ip: {{ mc01_out.tagged_instances[0].private_ip }}
          db01_ip: {{ db01_out.tagged_instances[0].private_ip }}

    - name: Copy login key to provision_stack directory
      copy:
        src: loginkey_vpro.pem
        dest: provision-stack/loginkey_vpro.pem
        mode: '0400'

    - name: Insert/Update Inventory file provision-stack/inventory-vpro
      blockinfile:
        path: provision-stack/inventory-vpro
        block: |
          web01 ansible_host={{ web01_out.tagged_instances[0].private_ip }}
          app01 ansible_host={{ app01_out.tagged_instances[0].private_ip }}
          rmq01 ansible_host={{ rmq01_out.tagged_instances[0].private_ip }}
          mc01 ansible_host={{ mc01_out.tagged_instances[0].private_ip }}
          db01 ansible_host={{ db01_out.tagged_instances[0].private_ip }}
          cntl ansible_host=127.0.0.1 ansible_connection=local

          [websrvgrp]
          web01

          [appsrvgrp]
          app01

          [rmqsrvgrp]
          rmq01

          [mcsrvgrp]
          mc01

          [dbsrvgrp]
          db01

          [control]
          cntl

          [stack_inst:children]
          websrvgrp
          appsrvgrp
          rmqsrvgrp
          mcsrvgrp
          dbsrvgrp

          [stack_inst:vars]
          ansible_user=ubuntu
          ansible_ssh_private_key_file=loginkey_vpro.pem
          #ansible_python_interpreter=/usr/bin/python3

   ```
<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>



## :page_facing_up: Resources

<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>


## :star2: Acknowledgment


<br/>
<div align="right">
    <b><a href="#Project-11">↥ back to top</a></b>
</div>
<br/>
