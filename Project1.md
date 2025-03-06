# PROJECT TITLE 1 : 
## AUTOMATING THE DEPLOYMENT AND MANAGEMENT OF ELITE CORPORATION'S  WEB-SERVER AND DATABASE SERVER USING ANSIBLE

### PROJECT FOCUS : 
Installation of Nginx on Elite’s corperations Web servers and Apache on their database servers using Ansible playbooks and roles:

The project  involves Setting up five EC2 instances on AWS, The Ansible server will be called the controller server and 2 web servers ( webserver1 and 2), 2 databaseserver (1 and 2). The controller is the central (main) server from which Ansible will manage the other instances. The web servers will host Nginx, while the database servers will host Apache.
Playbooks are written to define the tasks to be executed on webserver the (WBS) and database server (DBS).The playbooks are executed from the control node. Ansible connects to the web and database servers via SSH and performs the tasks defined in the playbooks.  After the playbooks are executed, the controller terminal  verifies that Nginx and Apache are installed and running on the respective servers.


-----
## PROJECT TASK:

##  LAUNCH AWS EC2 INSTANCES FOR FIVE SERVERS

- I Logged into the AWS Management Console, navigated to the EC2 dashboard , click on instance to Launch the 5 Instances as shown below.

![pic](<img/Screenshot (830).png>)
![pic](<img>)

### Public IPs for the Five(5) servers

- Controller: 34.211.231.74
- Webserver1: 18.237.97.46
- Webserver2: 35.160.214.69
- DBS 1: 35.91.18.227
- DatabaseSever2: 35.90.5.52

## INSTALLATION OF ANSIBLE ON THE CONTROLLER TERMINAL
- I started by connected through SSH into the controller terminal using the key pair (AWS1.pem) as shown below

![pic](<img/Screenshot (768).png>)




- Next is to Install Ansible on the Controller terminal  by first Updating the package list and then installing Ansible using the commands below


$ sudo apt update
$ sudo apt upgrade
$ sudo apt install ansible

![pic](<img/Screenshot (842).png>)
![pic](<img/Screenshot (841).png>)


##  GENERATING THE PRIVATE AND PUBLIC KEY

- I started by generating SSH keys using the ssh-keygen command for the WBS 1 & 2, DWB 1 & 2.
$ ssh-keygen. 
- enter the command ls ~/.ssh to show keys that are within the terminal- authorized_keys, private and public key.
- then command vi ~/.ssh/authorized_keys.



![pic](<img/Screenshot (806).png>)

- The generated public key was updated in the target server's using the authorised public key
![pic](<img/Screenshot (827).png>)

![pic](<img/Screenshot (828).png>)

![pic](<img/Screenshot (832).png>)


![pic](<img/Screenshot (833).png>)

![pic](<img/Screenshot (817).png>)

![pic](<img/Screenshot (819).png>)


![pic](<img/Screenshot (820).png>)

![pic](<img/Screenshot (823).png>)



- I verified using the public IP address of webserver and database servers by (ssh IP address)

![pic](<img/Screenshot (812).png>)

## INVENTORY FILE CREATION AND DEFINING OF SERVERS


- I created a new file in Ansible folder named Inventory using the touch command 

![pic](<img/Screenshot (814).png>)

- The file was edited  to define the inventory using the vi command adding the contents below

Groups servers for targeted playbook execution.

[WBS]
18.237.97.46
35.160.214.69

[DBS]
35.91.18.227
35.90.5.52

![pic](<img/Screenshot (813).png>)

##  WEB SERVER (WBS) AND DATABASE SERVER (DBS) PLAYBOOK CREATED 

- a new file named WBS.yml and DBS.yml was created to define the playbook using the touch command as shown below

![pic](<img/Screenshot (814).png>)


- I edited it and added the following contents to the WBS.yml to define the playbook as shown below

![pic](<img/Screenshot (816).png>)

- I edited it and added the following contents to the DBS.yml to define the playbook as shown below



![pic](<img/Screenshot (818).png>)



## ANSIBLE PLAYBOOK EXECUTION

- I executed the WBS.yml playbook and DBS.yml using the command ansible-playbook -i inventory WBS.yml and DBS.yml

![pic](<img/Screenshot (821).png>)

![pic](<img/Screenshot (822).png>)

![pic](<img/Screenshot (824).png>)


##  DEPLOYMENT VERIFICATION

- I verify that the playbooks executed successfully by checking the output of the ansible-playbook command
- I Verify that the Nginx and Apache services are running on the web servers and database servers, respectively using the command below

$ systemctl status nginx
$ systemctl status apache2

![pic](<img/Screenshot (834).png>)

![pic](<img/Screenshot (836).png>)

- Web Server (WBS)

![pic](<img/Screenshot (838).png>)

- Database Server (DBS)

![pic](<img/Screenshot (839).png>)





---
---

#### The End Of Project 1